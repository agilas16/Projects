pipeline {
	agent any

	jdk = tool name: 'JDK 11.0.7'
	env.JAVA_HOME = "${jdk}"
	
	stages {
		stage('checkout'){
		    steps{
		        git 'https://github.com/agilas16/Projects/tree/master/nunit-hello-world-master'
		    }
	    }
		stage('Build + SonarQube analysis') {
			def sqScannerMsBuildHome = tool 'SonarScannerMSBuild'
			withSonarQubeEnv(credentialsId: 'JenkinsM13SonarToken', installationName: 'M13 SonarQube') {
			bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe begin /k:"nunit_opencover" /d:sonar.host.url="http://10.142.146.158:9000" /d:sonar.login="be97ae33465186d7c92155f69fe8a4762654e5d1" /d:sonar.cs.nunit.reportsPaths=NUnitResults.xml /d:sonar.cs.opencover.reportsPaths=OpenCoverResults.xml"
			bat 'MSBuild.exe /t:Rebuild'
			bat 'packages\NUnit.ConsoleRunner.3.11.1\tools\nunit3-console.exe --result=NUnitResults.xml "NUnitExamples.Tests\bin\Debug\NUnitExamples.Test.dll"'
			bat 'packages\OpenCover.4.7.922\tools\OpenCover.Console.exe -output:OpenCoverResults.xml -register:user -target:"packages\NUnit.ConsoleRunner.3.11.1\tools\nunit3-console.exe" -targetargs:"NUnitExamples.Tests\bin\Debug\NUnitExamples.Test.dll"'
			bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe end"
		}
		
    }
}