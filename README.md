# IPC Explorer

Aplicación de escritorio para entender y gestionar la comunicación entre procesos en Windows y la pila SMB: named pipes, puertos ALPC, RPC, sockets, el servidor y el cliente SMB, los recursos compartidos y sus políticas. Está hecha en C# con WPF y .NET 8, con tema oscuro.

La versión 2 parte del inventario en vivo de la primera y añade una consola de comandos, la visualización del flujo, la gestión real de protocolos, servicios y políticas, un explorador de recursos y un registro con copias de seguridad.

> Los cambios son reales. Las pestañas Rol y servicios, Protocolos SMB y Políticas modifican el registro, los servicios, las características de Windows y el firewall del equipo donde se ejecuta. Antes de cada cambio se guarda una copia de seguridad para poder deshacerlo. Úsala en una máquina de pruebas.

## Compilar y ejecutar

Necesitas Windows 10/11 o Server 2016 o posterior de 64 bits, el [SDK de .NET 8](https://dotnet.microsoft.com/download/dotnet/8.0) y acceso a nuget.org la primera vez, para descargar el paquete `NtApiDotNet`.

```powershell
dotnet build -c Release
dotnet run -c Release --project src\IpcExplorer
```

También puedes abrir `IpcExplorerV2.sln` en Visual Studio 2022. Para generar un ejecutable único que lleves a otro equipo (necesita el runtime de .NET 8 Desktop):

```powershell
dotnet publish src\IpcExplorer -c Release -r win-x64 --self-contained false -p:PublishSingleFile=true
```

## Privilegios de administrador

La aplicación arranca sin elevar. Como usuario estándar puedes ver el inventario, ejecutar los comandos de consulta y navegar por los recursos a los que tengas acceso. Para aplicar cualquier cambio, leer `Get-SmbServerConfiguration`, consultar las características de Windows, ver sesiones y archivos abiertos o ver los permisos de los recursos hace falta ser administrador.

Cuando algo lo necesita, la interfaz lo avisa y ofrece reiniciar como administrador (con el diálogo de UAC). La aplicación vuelve a abrirse en la misma pestaña.
