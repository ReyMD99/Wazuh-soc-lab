
Este proyecto documenta la implementacion de un SIEM/EDR como Wazuh con contenedores en **UBUNTU**

By: ReyMarte
## Entorno Utilizado

Host: Ubuntu
Software: Docker Engine y Docker compose


> [!NOTE] Puntos necesarios antes de proceder
> 	 - Recomiendo tambien el uso de docker desktop para mayor control visual de los contenedores. 
> 	 - Recomiendo contar con 16gb de ram para poder hacer este lab sin problemas 		


### Despliegue paso a paso

#### - Preparacion del entorno.
Antes de desplegar es necesario ajustar el limite de memoria virtual del kerner para que Wazuh indexer funcione correctamente, esto se emplea con:
```
sudo sysctl -w vm.max_map_count=262144
# Para pque sea persistente:
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
```
#### Clonar repo y generar certificados TLS(Seguridad)
Para Wazuh recomiendo una buena practica cifrando la comunicacion entre sus nodos. EL uso de certificados como TLS es basicamente obligatorio justo despues de clonar el repositorio. Esto se ejecuta con los siguientes comandos:

```
# Clonar repo y crear carpeta directa para trabajar con el
git clone https://github.com/wazuh/wazuh-docker.git -b v4.10.1 --depth=1
cd wazuh-docker/single-node

#O en todo caso este mismo repositorio, se deja a disposicion de usted.

# Ejecutar generador de certificados
docker-compose -f generate-indexer-certs.yml run --rm generator
```
#### Ejecucion de los servicios de Wazuh(Stack)
Para levantar los 3 servicios(Indexer, manager y el Dashboard) se usa el comando de docker:
```
docker-compose up -d
```
#### Pasos finales
Se recomienda postumo al docker-compose, ejecutar asi mismo el comando:
```
Docker ps
```
Con eso se podra ver si todos los contenedores se levantaron con exito. Es preferible esperar de 1 a 2 minutos a que cargue todo y ya asi utilizar los siguientes parametros en su navegador:

**URL**: https://localhost:443
**Usuario**: admin
Password: SecretPassword o SecretPassword01!


