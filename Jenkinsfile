pipeline {
    agent any

    environment {
        APP_NAME = 'Belajar Jenkins'
        DEVELOPER = 'Aditia Pratama'
        VERSION = '1.0.0'
        BACKUP_DIR = '/tmp/backup'
    }

    stages {

        stage('Persiapan') {
            steps {
                echo '==============================='
                echo '   MEMULAI PIPELINE BUILD      '
                echo '==============================='
                echo "Aplikasi  : ${APP_NAME}"
                echo "Developer : ${DEVELOPER}"
                echo "Versi     : ${VERSION}"
                echo "Branch    : ${env.GIT_BRANCH}"
                echo "Build No  : ${env.BUILD_NUMBER}"
            }
        }

        stage('Checkout') {
            steps {
                echo '--- Mengambil kode dari repository ---'
                checkout scm
                echo 'Kode berhasil di-checkout!'
            }
        }

        stage('Build') {
            steps {
                echo '--- Memulai proses Build ---'
                echo "Membangun ${APP_NAME} versi ${VERSION}..."
                echo 'Build selesai!'
            }
        }

        stage('Test') {
            steps {
                echo '--- Menjalankan Testing ---'
                echo 'Menjalankan unit test...'
                echo 'Semua test berhasil!'
            }
        }

        stage('Backup') {
            steps {
                echo '--- Memulai proses Backup ---'
                sh '''
                    TIMESTAMP=$(date +%Y%m%d_%H%M%S)
                    BACKUP_PATH=/tmp/backup/$TIMESTAMP
                    mkdir -p $BACKUP_PATH
                    echo "Backup dibuat di: $BACKUP_PATH"

                    # Backup workspace
                    cp -r $WORKSPACE $BACKUP_PATH/workspace_backup

                    echo "Isi folder backup:"
                    ls -lh $BACKUP_PATH

                    echo "Backup selesai pada: $TIMESTAMP"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '--- Deployment ---'
                echo "Mendeploy ${APP_NAME} ke server..."
                sh '''
                    echo "Verifikasi backup tersedia sebelum deploy..."
                    ls /tmp/backup/
                    echo "Backup terverifikasi, melanjutkan deploy..."
                    echo "Deploy selesai!"
                '''
            }
        }

    }

    post {
        success {
            echo '==============================='
            echo '   BUILD BERHASIL! SUCCESS     '
            echo '==============================='
        }
        failure {
            echo '==============================='
            echo '   BUILD GAGAL! FAILURE        '
            echo '==============================='
        }
        always {
            echo "Pipeline selesai - Build #${env.BUILD_NUMBER}"
            sh 'echo "Daftar semua backup:" && ls /tmp/backup/ || true'
        }
    }
}
