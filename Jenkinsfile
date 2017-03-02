//Android Jenkinsfile

pipeline {

	agent any
	
	stages {

		stage ('Building') {
			steps {
				sh '''
					echo building...
					./gradlew clean
					./gradlew build
				'''
			}
		} 

		stage ('Upload to Veracode') {
			steps {
				sh '''
					#get the commit message
					#if developer did not request a scan then there is no need to do anything
					commit_message=$(git log -n 1 --pretty=%s)
					#search for the scan keyword within the commit message
					if echo "$commit_message" | grep -q "Veracode=True"
					then
						echo "Scan requested, uploading to veracode"
						echo "True" > scan.txt
					else
						echo "No Scan requested, will not upload to veracode"
						echo "False" > scan.txt
					fi
					#echo $BUILD_NUMBER
					#echo $BRANCH_NAME
					#Check path
					for FILENAME in "tictactoe/*.apk"
						do PATHNAME=tictactoe/
						FILEPATHNAME=$FILENAME"_"$BRANCH_NAME
               			FILENAME=${FILEPATHNAME#$PATHNAME}
		                echo $FILENAME
		                echo Saving the following name in an output file
		                echo $FILENAME > output_file_name.txt
					done
					'''


				// save the apk file name in an environment variable
				script {
					env.FILENAME = readFile 'output_file_name.txt'
					env.TEXT = readFile 'scan.txt'

					if (env.TEXT.contains("True")) {
						echo "Read to scan!"
						veracode applicationName: 'UGO - UGO Digital Wallet - Android', createProfile: false, criticality: 'VeryHigh', fileNamePattern: '', pHost: '', pPassword: '', pUser: '', replacementPattern: '', sandboxName: 'Sandbox Testing Sakib3', scanExcludesPattern: '', scanIncludesPattern: '', scanName: "${BRANCH_NAME} ${env.FILENAME} ${timestamp}", uploadExcludesPattern: '', uploadIncludesPattern: '**/**.jar', useIDkey: true, vid: 'bff67dd63d41f4331068e44ae216bbe4', vkey: 'e78160940b40a58ec04001889062e577007516ae8387e684ed2b99a8bba6bdc07a2366f8d127fd51b59bb52848ca6c19c835a99741507a002a76fda1191f5153', vpassword: '', vuser: ''
					} else {
						echo "Scan is a no go"
					}
				}

				echo "Content in the scan.txt"
				echo "${env.TEXT}"
				
				
				sh '''
					rm output_file_name.txt
					rm scan.txt
				'''
			}

		}
	}
}