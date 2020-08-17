pipeline {
    agent none

    stages{
        stage('Rgression Tests'){
				agent { label 'yocto-test-sc584'}
				steps {
					sh '''
						cd $WORKSPACE
						source ./setup-environment -m adsp-sc573-ezkit -b build
						cd $WORKSPACE/build/conf
						grep -q 'adsp-sc573-ezkit' local.conf; [ $? -eq 0 ] && echo "Pass" || exit 1
						'''
					sh '''
						cd $WORKSPACE
						source ./setup-environment -m adsp-sc584-ezkit
						cd $WORKSPACE/build/conf
						grep -q 'adsp-sc584-ezkit' local.conf; [ $? -eq 0 ] && echo "Pass" || exit 1
						'''
					sh '''
						cd $WORKSPACE
						source ./setup-environment -b build
						cd $WORKSPACE/build/conf
						grep -q 'adsp-sc584-ezkit' local.conf; [ $? -eq 0 ] && echo "Pass" || exit 1
						'''
					sh '''
						cd $WORKSPACE/sources/load-uboot-kernel
						cp ~/PW_RESET.CFG ./PW_RESET.CFG 
						echo test | sudo -S python3 LUK.py -b nfsboot -m adsp-sc584-ezkit --ipaddr 10.100.4.50 --serverip 10.100.4.174 -f //dte-shanghai/share/jenkins/app-yocto-build/u-boot/adsp-sc584-ezkit --updateUboot -e 1000
						'''
					sh '''
						cd $WORKSPACE/sources/load-uboot-kernel
						echo test | sudo -S python3 LUK.py -b nfsboot -m adsp-sc584-ezkit --ipaddr 10.100.4.50 --serverip 10.100.4.174
						'''
					sh '''
						cd $WORKSPACE/sources/load-uboot-kernel
						echo test | sudo -S python3 LUK.py -m adsp-sc584-ezkit --updateUboot -e 1000 --ipaddr 10.100.4.50 --serverip 10.100.4.174
						'''
				}
        }
    }
}