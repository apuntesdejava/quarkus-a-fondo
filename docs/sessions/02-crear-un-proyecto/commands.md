# Comandos — Sesión 02

## Crear la demo

Desde la raíz del repositorio:

```powershell
Set-Location demos
```

Crear el proyecto:

```powershell
mvn "io.quarkus.platform:quarkus-maven-plugin:3.40.1.1:create" `
    "-DprojectGroupId=io.github.apuntesdejava.quarkusafondo" `
    "-DprojectArtifactId=02-project-structure" `
    "-Dextensions=rest"
```

Entrar al proyecto:

```powershell
Set-Location .\02-project-structure
```

## Verificar Maven Wrapper

```powershell
.\mvnw.cmd -version
```

## Listar extensiones instaladas

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```

## Agregar una extensión temporal

```powershell
mvn quarkus:add-extension "-Dextensions=smallrye-health"
```

Comprobar nuevamente las extensiones instaladas:

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```

## Eliminar la extensión temporal

```powershell
mvn quarkus:remove-extension "-Dextensions=smallrye-health"
```

## Revisar el Issue Tracker

Desde la raíz del repositorio:

```powershell
Set-Location issue-tracker
```

Listar sus extensiones instaladas:

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```
