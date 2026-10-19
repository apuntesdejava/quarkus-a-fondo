# Comandos — Sesión 01

Los comandos siguientes aparecen en el orden necesario para preparar y reproducir la sesión. El entorno base es **JDK 25 + Maven 3.9.16 + Git**.

## 1. Preparar Java en Windows

Descargar el script publicado para el curso:

```powershell
Invoke-WebRequest -Uri "https://gist.githubusercontent.com/diegosilval/8ea70b4ba5c92bebcd70057c2559bb33/raw" -OutFile "setup-java.ps1"
```

Ejecutarlo indicando explícitamente JDK 25:

```powershell
.\setup-java.ps1 -JavaVersion 25
```

Comprobar la instalación:

```powershell
java -version
javac -version
```

## 2. Preparar Maven en Windows

Descargar el script publicado para el curso:

```powershell
Invoke-WebRequest -Uri "https://gist.githubusercontent.com/diegosilval/897f9f5d9cae0e3f78d315433944403d/raw" -OutFile "setup-mvn.ps1"
```

Ejecutarlo indicando siempre la versión utilizada por el curso:

```powershell
.\setup-mvn.ps1 -MavenVersion 3.9.16
```

Comprobar Maven y el JDK que utiliza:

```powershell
mvn -version
```

## 3. Verificar Git

```powershell
git --version
```

## 4. Crear la demo aislada

Desde la raíz del repositorio:

```powershell
Set-Location demos
```

Crear el proyecto:

```powershell
mvn io.quarkus.platform:quarkus-maven-plugin:3.40.1:create "-DprojectGroupId=io.github.apuntesdejava.quarkusafondo" "-DprojectArtifactId=01-getting-started" "-Dextensions=rest"
```

Entrar en la demo:

```powershell
Set-Location 01-getting-started
```

Ejecutar la aplicación:

```powershell
mvn quarkus:dev
```

Con la aplicación en ejecución, comprobar el endpoint desde otra terminal:

```powershell
Invoke-WebRequest -Uri "http://localhost:8080/hello" | Select-Object -ExpandProperty Content
```

## 5. Crear el Issue Tracker

Detener la demo y volver a la raíz del repositorio.

```powershell
Set-Location ../..
```

Crear el proyecto transversal:

```powershell
mvn io.quarkus.platform:quarkus-maven-plugin:3.40.1:create "-DprojectGroupId=io.github.apuntesdejava.quarkusafondo" "-DprojectArtifactId=issue-tracker" "-Dextensions=rest"
```

Entrar en el proyecto:

```powershell
Set-Location issue-tracker
```

Ejecutarlo:

```powershell
mvn quarkus:dev
```

Comprobar el endpoint desde otra terminal:

```powershell
Invoke-WebRequest -Uri "http://localhost:8080/hello" | Select-Object -ExpandProperty Content
```
