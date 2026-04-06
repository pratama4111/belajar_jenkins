pipeline {
    agent any
    environment {
        APP_NAME = 'Belajar Jenkins'
        DEVELOPER = 'Aditia Pratama'
        VERSION = '1.0.0'
        BACKUP_DIR = '/tmp/backup'
        EMAIL_TO = 'pratamaaditia938@gmail.com'
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
                    BACKUP_PATH=/tmp/backup/$TIMESTAMP/workspace_backup
                    mkdir -p "$BACKUP_PATH"
                    echo "Backup dibuat di: $BACKUP_PATH"
                    cp -r "$WORKSPACE/." "$BACKUP_PATH/"
                    echo "Isi folder backup:"
                    ls -lh "$BACKUP_PATH"
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
        always {
            script {
                def status = currentBuild.result ?: 'SUCCESS'
                def emoji = status == 'SUCCESS' ? 'BERHASIL' : 'GAGAL'
                def pesan = status == 'SUCCESS'
                    ? 'Build pipeline berhasil dijalankan!'
                    : 'Build pipeline GAGAL! Segera periksa.'

                echo '==============================='
                echo "   BUILD ${emoji}!             "
                echo '==============================='

                mail to: "${EMAIL_TO}",
                     subject: "${status}: Build #${env.BUILD_NUMBER} - ${APP_NAME}",
                     body: """
Halo ${DEVELOPER},

${pesan}

==============================
INFO BUILD
==============================
Aplikasi  : ${APP_NAME}
Versi     : ${VERSION}
Build No  : ${env.BUILD_NUMBER}
Branch    : ${env.GIT_BRANCH}
Status    : ${status}

Cek detail: ${env.BUILD_URL}
==============================

Salam,
Jenkins CI/CD
                     """

                sh 'echo "Daftar semua backup:" && ls /tmp/backup/ || true'
            }
        }
    }
}
