pipeline {
    agent none

    stages{
        stage('Rgression Tests'){
				agent { label 'yocto-test-sc584'}
				steps {
					sh '''
						#!/bin/bash
						cd $WORKSPACE
						. ./setup-environment -m adsp-sc573-ezkit -b build
						cd $WORKSPACE/build/conf
						grep -q 'adsp-sc573-ezkit' local.conf; [ $? -eq 0 ] && echo "Pass" || exit 1
						'''
					sh '''
						#!/bin/bash
						cd $WORKSPACE
						source setup-environment -m adsp-sc584-ezkit
						cd $WORKSPACE/build/conf
						grep -q 'adsp-sc584-ezkit' local.conf; [ $? -eq 0 ] && echo "Pass" || exit 1
						'''
					sh '''
						#!/bin/bash
						cd $WORKSPACE
						source setup-environment -b build
						cd $WORKSPACE/build/conf
						grep -q 'adsp-sc584-ezkit' local.conf; [ $? -eq 0 ] && echo "Pass" || exit 1
						'''
				}
        }
    }
}