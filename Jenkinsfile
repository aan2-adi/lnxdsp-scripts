pipeline {
    agent none

    stages{
        stage('Rgression Tests'){
				agent { label 'yocto-test-sc573'}
				steps {
					sh  '''
						# define the yocto releated bibucket branch
						echo ${GIT_BRANCH}
						export manifest_branch="develop/yocto-1.0.1"
						export meta_adi_branch="develop/yocto-1.0.1"
						export kernel_branch="develop/yocto-1.0.1"
						export script_branch=${GIT_BRANCH}
						export uboot_branch="develop/yocto-1.0.1"

						cd $WORKSPACE
						if [ -e sources ]; then
							rm -rf sources
						fi
						if [ -e build ]; then
							rm -rf build
						fi
						if [ -e $WORKSPACE/bin ]; then
							rm -rf bin
							rm -rf .repo
						fi
						mkdir bin
						curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ./bin/repo
						echo test | sudo chmod a+x ./bin/repo
						./bin/repo init -u https://testlab2:Labrat1@bitbucket.analog.com/scm/dte/lnxdsp-repo-manifest.git -b ${manifest_branch}
						./bin/repo sync -f
						chmod a+x $WORKSPACE/.repo
						chmod a+x $WORKSPACE/.repo/.repo_fetchtimes.json

						########### Check all URIs to bitbucket ####################

						remotename=adibitbucket
						cd $WORKSPACE/sources;
						git remote add $remotename https://bitbucket.analog.com/scm/dte/lnxdsp-scripts.git;git remote remove adigithub;git remote update;git checkout ${script_branch}
						cd $WORKSPACE/sources/meta-adi;
						git remote add $remotename https://bitbucket.analog.com/scm/dte/lnxdsp-adi-meta.git; git remote remove adigithub;git remote update;git checkout ${meta_adi_branch}


						# ########### Update the local.conf file ###########

						chmod a+x $WORKSPACE
						cd $WORKSPACE/sources/base/$MACHINE
						# update repo URI
						echo 'KERNEL_GIT_URI ?= "git://bitbucket.analog.com/scm/dte/lnxdsp-linux.git"' >> local.conf
						echo 'UBOOT_GIT_URI ?= "git://bitbucket.analog.com/scm/dte/u-boot.git"' >> local.conf
						# Update branch for kernel and u-boot
						echo 'UBOOT_BRANCH ?= "'${uboot_branch}'"'>> local.conf
						echo 'KERNEL_BRANCH ?= "'${kernel_branch}'"'>> local.conf
						'''
					sh '''
						#!/bin/bash -ilex
						cd $WORKSPACE
						. ./setup-environment -m adsp-sc573-ezkit -b build
						cd $WORKSPACE/build
						bitbake u-boot-adi
						'''
					sh '''
						#!/bin/bash -ilex
						cd $WORKSPACE
						. ./setup-environment -m adsp-sc573-ezkit
						cd $WORKSPACE/build
						bitbake u-boot-adi
						'''
					sh '''
						#!/bin/bash -ilex
						cd $WORKSPACE
						. ./setup-environment -b build
						cd $WORKSPACE/build
						bitbake u-boot-adi
						'''
					sh '''
						#!/bin/bash -ilex
						cd $WORKSPACE/sources/load-uboot-kernel
						cp ~/PW_RESET.CFG ./PW_RESET.CFG 
						echo test | sudo -S python3 LUK.py -b nfsboot -m adsp-sc573-ezkit --ipaddr 10.100.4.50 --serverip 10.100.4.174 -f $WORKSPACE/build/tmp/deploy/images/adsp-sc573-ezkit --updateUboot -e 1000
						'''
					sh '''
						#!/bin/bash -ilex
						cd $WORKSPACE/sources/load-uboot-kernel
						echo test | sudo -S python3 LUK.py -b nfsboot -m adsp-sc573-ezkit --ipaddr 10.100.4.50 --serverip 10.100.4.174
						'''
					sh '''
						#!/bin/bash -ilex
						cd $WORKSPACE/sources/load-uboot-kernel
						echo test | sudo -S python3 LUK.py -m adsp-sc573-ezkit --updateUboot -e 1000 --ipaddr 10.100.4.50 --serverip 10.100.4.174
						'''
				}
        }
    }
}