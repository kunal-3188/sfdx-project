{\rtf1\ansi\ansicpg1252\cocoartf2580
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fnil\fcharset0 Monaco;}
{\colortbl;\red255\green255\blue255;\red22\green21\blue22;\red255\green255\blue255;}
{\*\expandedcolortbl;;\cssrgb\c11373\c10980\c11373;\cssrgb\c100000\c100000\c100000;}
\margl1440\margr1440\vieww13960\viewh14760\viewkind0
\deftab720
\pard\pardeftab720\sl390\partightenfactor0

\f0\fs26 \cf2 \cb3 \expnd0\expndtw0\kerning0
#!groovy\cb1 \
\cb3 pipeline \{ \cb1 \
   \cb3 agent any\
   tool \{ sfdx\
   \}\
\
		stages \{\
			stage(\'91git check-out\'92) \{\
\cb1 				steps \{\
					git \'91\cb3 https://github.com/kunal-3188/sfdx-project.git\cb1 \'92\
					 \}\
				\}\
\
\
\cb3 			stage(\'91salesforce Deploy) \{\cb1 \
\cb3             		steps \{\cb1 \
\cb3                 		authSF()\cb1 \
\cb3                 		salesforceDeploy()\cb1 \
\cb3                 		\}\cb1 \
\cb3             		\}\cb1 \
\cb3         \}\cb1 \
\}\
\
\cb3 def salesforceDeploy() \{\cb1 \
\cb3     \cb1 \
\cb3     def varsfdx = tool 'sfdx'\cb1 \
\cb3     rc2 = command "$\{varsfdx\}/sfdx force:auth:sfdxurl:store -f authjenkinsci.txt -a targetEnvironment"\cb1 \
\cb3     if (rc2 != 0) \{\cb1 \
\cb3        echo " 'SFDX CLI Authorization to target env has failed.'"\cb1 \
\cb3     \}\cb1 \
\
\cb3 def authSF() \{\cb1 \
\cb3     echo 'SF Auth method'\cb1 \
\cb3     def SF_AUTH_URL\cb1 \
\cb3     echo env.BRANCH_NAME\cb1 \
\
\cb3     if ("$\{currentBuild.buildCauses\}".contains("UserIdCause")) \{\cb1 \
\cb3         def fields = env.getEnvironment()\cb1 \
\cb3         fields.each \{\cb1 \
\cb3             key, value -> if("$\{key\}".contains("$\{params.target_environment\}")) \{ SF_AUTH_URL = "$\{value\}"; \}\cb1 \
\cb3         \}\cb1 \
\cb3     \}\cb1 \
\cb3     else if("$\{currentBuild.buildCauses\}".contains("BranchEventCause")) \{\cb1 \
\cb3         if(env.BRANCH_NAME == 'master' || env.CHANGE_TARGET == 'master') \{\cb1 \
\cb3             SF_AUTH_URL = env.SFDX_DEV\cb1 \
\cb3         \}\cb1 \
\cb3         else \{ // \{PR\} todo - better determine if its a PR env.CHANGE_TARGET?\cb1 \
\cb3             SF_AUTH_URL = env.SFDX_DEV\cb1 \
\cb3         \}\cb1 \
\cb3     \}\cb1 \
\
\cb3     echo SF_AUTH_URL\cb1 \
\cb3     writeFile file: 'authjenkinsci.txt', text: SF_AUTH_URL\cb1 \
\cb3     sh 'ls -l authjenkinsci.txt'\cb1 \
\cb3     sh 'cat authjenkinsci.txt'\cb1 \
\cb3     echo 'end sf auth method'\cb1 \
\cb3 \}\cb1 \
}