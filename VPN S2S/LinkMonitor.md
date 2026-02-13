# Túneles S2S Activo-Pasivo en Fortinet

- Esta es la forma antigua que se utilizaba para montar túneles S2S con líneas de respaldo (activo-pasivo)-
- Ahora se utiliza una SDWAN pero vamos a ver el método clásico 
- El túnel principal tendrá en su ruta estática una Distancia Administrativa Menor que la de backup (10)
- El túnel de backup tendrá en su ruta estática una Distancia Administrativa Mayor que la línea principal (20)
- El fortigate enviará el tráfico principalmente por la línea principal (activa) a causa de su DA menor.
- La línea de backup aparecerá "caída" porque el tráfico circulará por la línea principal
- Cuando la línea principal se caida, el fortigate pivotará el tráfico hacia la línea de backup.
- Para detectar que la línea principal esté caída hay que utilizar lo que se llama como LINK HEALTH MONITOR
- Comando en Fortigate para ver la tabla de enrutamiento con las rutas activas. La línea de backup no debe aparecer
porque no está activa. Cuando el túnel principal se caiga, la ruta de backup se mostarará en la tabla

`get router info routing-table all`

- El health monitor lo que hace es mandar pings a un host del otro túnel, por ejemplo
la interfaz LAN del otro extremo. Cuando no obtengamos respuesta por parte del otro extremo, 
entenderemos que la línea principal no funciona y la línea de backup pasará a ser la activa 
hasta que la línea principal se restablezca. Es por eso que usamos las distancias administrativas.

Más comandos útiles


`config vpn ipsec phase1-interface`
`edit "vpn_s2s_backup"`
`set dpd on-idle`
`set monitor "vpn_s2s_activa"`
`end`
