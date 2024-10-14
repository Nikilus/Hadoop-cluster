pipeline {
    agent any
    tools {
        maven 'Maven'  
        jdk 'JAVA 8'   
    }

    environment {
        PROTOBUF_INCLUDE_DIR = '/usr/include'
        PROTOBUF_LIBRARY = '/usr/lib/x86_64-linux-gnu/libprotobuf.so'
        PROTOC_LIBRARY = '/usr/bin/protoc'
        GENERATED_JAVAH = '${WORKSPACE}/hadoop-hdfs-project/hadoop-hdfs-native-client/target/native/javah'
    }

    stages {
        stage('Checkout') {
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    git branch: 'branch-3.3.5', url: 'https://github.com/apache/hadoop.git'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    //  CMake
                    sh '''
                        cmake ${WORKSPACE}/hadoop-hdfs-project/hadoop-hdfs-native-client/src \
                            -DProtobuf_INCLUDE_DIR=${PROTOBUF_INCLUDE_DIR} \
                            -DProtobuf_LIBRARY=${PROTOBUF_LIBRARY} \
                            -DProtobuf_PROTOC_LIBRARY=${PROTOC_LIBRARY} \
                            -DGENERATED_JAVAH=${GENERATED_JAVAH} \
                            -DHADOOP_BUILD=1 \
                            -DJVM_ARCH_DATA_MODEL=64 \
                            -DREQUIRE_FUSE=false \
                            -DREQUIRE_LIBWEBHDFS=false \
                            -DREQUIRE_VALGRIND=false \
                            -G "Unix Makefiles"
                    '''

                    // Компиляция с помощью make
                    sh 'make'

                    // Сборка через Maven
                    sh 'mvn clean install -Pdist,native -Drequire.fuse -Dtar -DskipTests'
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}

