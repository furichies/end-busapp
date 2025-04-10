// Jenkinsfile
pipeline {
    // Especifica que el pipeline completo debe ejecutarse en el agente con la etiqueta 'aula11.chikenkiller.com'
    // Asegúrate de que esta etiqueta coincida con la configurada en Jenkins para tu nodo agente.
    agent { label 'aula11.chickenkiller.com' }

    stages {
        stage('Checkout: Clonar Repositorio') {
            steps {
                echo "Limpiando el workspace..."
                // Limpia el workspace para asegurar que no haya archivos de ejecuciones anteriores
                cleanWs()

                echo "Clonando el repositorio https://github.com/tecducatio/end-busapp.git ..."
                // Clona el repositorio público. Asume la rama 'main', cámbiala si es otra (ej. 'master')
                git url: 'https://github.com/tecducatio/end-busapp.git', branch: 'main'
            }
        }

        stage('Deploy: Ejecutar Docker Compose') {
            steps {
                echo "Intentando detener y eliminar contenedores previos (si existen)..."
                // Ejecuta docker-compose down para limpiar cualquier instancia anterior.
                // --remove-orphans elimina contenedores de servicios que ya no están en el yml.
                // `|| true` evita que el pipeline falle si no hay nada que detener.
                sh 'docker compose down --remove-orphans || true'

                echo "Levantando la aplicación con docker-compose..."
                // Ejecuta docker-compose up en modo detached (-d)
                sh 'docker compose up -d'

                echo "Aplicación desplegada con Docker Compose."
            }
        }
    }

    post {
        // Bloque 'always' se ejecuta siempre al final, independientemente del resultado (éxito o fallo)
        always {
            echo "Ejecución del pipeline finalizada."
            // Opcional: Podrías añadir un 'docker-compose down' aquí si quieres limpiar siempre después de cada ejecución,
            // aunque para un despliegue continuo normalmente querrás que la app siga corriendo.
            // sh 'docker-compose down --remove-orphans || true'
        }
        success {
            echo "Pipeline ejecutado con éxito."
        }
        failure {
            echo "Pipeline ha fallado."
        }
    }
}
