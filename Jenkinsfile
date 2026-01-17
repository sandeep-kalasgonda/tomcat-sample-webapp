pipeline {
    agent any

    environment {
        TOMCAT_HOME = "/usr/share/tomcat"
        EC2_HOST = "18.189.194.104"
        SSH_CREDENTIALS = "-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA6lHTGcAgbvo+CJXL9AXb83LIxyV/KITNODBRNo1I199DnrLm
XZ8e9PgxkwLx5AKErcD9oLinyPx3hSV4mPula9/PSCdTAXZyDct4jYO4G1lcdjsn
DzP51c907D3rATD4qKilVuQJKMZdUphzg+lJcmbvwvtG1zJ2cXvk8+vsIm8Xixa4
xn/BcsunIVQ8NfHLq4dqB0WHtqpoMJPcYFHdl94VY58TcyapIM5u42Eh85KGfNne
8BMgqETEQrhiMocZ+V9J7GvuMSLj7IhSYt1U1x5p/jQzabO5SwiTSbJDI8yVk0zl
mSqz0gYMgC/rSYcuQjwiuhj/GhIRWhoXJk66YwIDAQABAoIBAQDpHkjoOIXMAziO
MAG+H8oo1Qy9XCe69wx4l1Fk9YEAC8Zpb9DSWclhrD8d5HMlBgBcIUHzkWKUCeEa
3SGzCcEUppDBVyY0sVNdOA4StHYI94mOsuD0NiCbwA6yLhPMlpz8pvP/k1UtdNRJ
pRRfq0t//rsQgA+Fb1X5J2qr5g6CXZ4JdFRHu6kbISSA/HtWhQUBUyCwi11NSMnv
zxAJjVsVToZjc+TXQCGiPXSroDyH3lSLqV6OS8dkScv1nw5e0DdE2mZVG32i/vOn
lhyj/LHhunPLspU33gwd5BFlYjK73p3hq9tp/hU148IiZ0Y0mBVj41ffp02GYN3Q
fAAnkN4BAoGBAP9uN96LeQwcuake6/D0ryyDLBAg7LwSKHYMfV+DtWCQ9tL39ylp
pcEbvB49QBH3s9BFw36kX7On13GdgYFU2E2KeD2L1cWJ53tqAgnliMIreR+PxhgN
w6yyUouyKHAwAFpgf53+AVrc8GEIHCC1kfVDHX/r+o9pfj9ODX37OU8BAoGBAOrX
jsi4DF7oLfDzMGTHqSPJki6jD7EiK0uHdoeyLI/s+LeR+sgvDxQufWznBRIyqY08
eIF9eTkmgfdpaJnxAH5BiAHSRBiFots7HBBDp8OqVj53n8JrGaAEsCSaDIPbObm7
F+QoLGC4RR6Jfiwtz0NWrnxItq++t+8cu52OQi1jAoGBAKdi0APrfEiermAQnmdJ
wV23G/H50YkxkQhDCQnFot+EP+tiibq+u9t/VFiwpMLhgxlSDll4WCrAK6QNpmdd
dV3jBwa2E0GfLG2ou2tG2sb7fCVdr1/l7TvHo+ZdurhCDIktQZQEd1jW/kNn8B7T
PbHu6G8C8jB23j+X46mSLy0BAoGAB1w2J2hNSvQv7GtSyvXPAUYiBMArj7uoa7eV
KW+WIfSlXut+VqPS7yj92VnsOMPJuJl6lWRfVkE0tZJiKuD4yPw4zQXQCIy3q/NQ
T9ou+dzu0wpgwXEl3nQHKT6CwecvCfkpKIdxzJ453Fkm0S+mXU/sLA0DXMK3dREL
eEarIE0CgYBtNqjDk5v5jMuORE01oDIbDL6dgJNPHIXuiJFBlW+eO+fMqzPziT8d
+32bhmjLKHYc3yKdjAqfBqX5/f373/FMpyX8Uj8TcqCrsZp0MA8uD6woaeb1mPTI
M63KluahcR2OYSQHYVtVD0RqvUAsqyeQF26GtZ+aEYyxSlwCtUnHYA==
-----END RSA PRIVATE KEY-----"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sandeep-kalasgonda/tomcat-sample-webapp.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to EC2 Tomcat') {
            steps {
                sshagent(credentials: [SSH_CREDENTIALS]) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/*.war ec2-user@${EC2_HOST}:${TOMCAT_HOME}/webapps/
                    ssh -o StrictHostKeyChecking=no ec2-user@${EC2_HOST} "
                      sudo systemctl restart tomcat
                    "
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully"
        }
        failure {
            echo "Deployment failed"
        }
    }
}
