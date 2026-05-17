# abmonzon32-max.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portafolio Técnico - Byron Armando Monzón Alman</title>
    <style>
        body { font-family: 'Segoe UI', Arial, sans-serif; line-height: 1.6; color: #333; max-width: 900px; margin: 0 auto; padding: 20px; background-color: #f7f9fa; }
        h1 { color: #1a365d; border-bottom: 2px solid #2b6cb0; padding-bottom: 10px; }
        h2 { color: #2b6cb0; margin-top: 30px; }
        h3 { color: #2d3748; }
        .project-card { background: white; padding: 25px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); margin-bottom: 30px; }
        .tech-tag { background: #e2e8f0; color: #4a5568; padding: 4px 10px; border-radius: 4px; font-size: 0.9em; font-weight: bold; margin-right: 5px; display: inline-block; }
        code { background: #edf2f7; padding: 2px 6px; border-radius: 4px; font-family: 'Courier New', Courier, monospace; }
        pre { background: #1a202c; color: #edf2f7; padding: 15px; border-radius: 6px; overflow-x: auto; font-family: 'Courier New', Courier, monospace; }
        .video-container { text-align: center; margin: 20px 0; background: #000; border-radius: 6px; padding: 10px; }
        .step-list { padding-left: 20px; }
        .step-list li { margin-bottom: 10px; }
    </style>
</head>
<body>

    <h1>Portafolio de Proyectos de Infraestructura y Redes</h1>
    <p><strong>Por:</strong> Byron Armando Monzón Alman | Telecommunications Engineer</p>

    <div class="project-card">
        <h2>Proyecto: Automatización y Hardening de Identidades con Active Directory (AD DS)</h2>
        <p><strong>Descripción:</strong> Despliegue de un entorno de red empresarial virtualizado sobre VMware Workstation utilizando Windows Server 2022 como Controlador de Dominio principal (<code>byronlab.local</code>) y hosts Windows 11 Enterprise como estaciones de trabajo corporativas. El proyecto se enfoca en la eficiencia operativa mediante la automatización por código y el endurecimiento de políticas de seguridad locales (Workstation Hardening).</p>
        
        <div>
            <span class="tech-tag">Windows Server 2022</span>
            <span class="tech-tag">Active Directory (AD DS)</span>
            <span class="tech-tag">PowerShell Automation</span>
            <span class="tech-tag">Group Policy Objects (GPOs)</span>
            <span class="tech-tag">VMware</span>
        </div>

        <h3>1. Arquitectura de Red y Laboratorio Virtual</h3>
        <ul class="step-list">
            <li><strong>Capa de Enlace y Red Virtual:</strong> Aislamiento de tráfico mediante segmentos LAN privados en VMware para emular switches de acceso locales separados de la red externa.</li>
            <li><strong>Servicios de Infraestructura:</strong> Configuración lógica de direccionamiento IP estático interno y roles de DNS integrados en Active Directory para resolver la zona jerárquica nativa.</li>
        </ul>

        <h3>2. Automatización con PowerShell (Creación de Estructura OU y Cuentas)</h3>
        <p>Para evitar tareas repetitivas mediante la interfaz gráfica, implementé un script en PowerShell para desplegar de forma inmediata la arquitectura de Unidades Organizativas (OUs) en inglés siguiendo estándares globales de IT corporativo, junto con el aprovisionamiento seguro de cuentas de usuario técnico:</p>
        
<pre>
# Inicialización de Estructura Base e Inserción de Usuarios
New-ADOrganizationalUnit -Name "Byron_Corp" -Path "DC=byronlab,DC=local"
$subOUs = @("Users", "Teams", "Groups")
foreach ($ou in $subOUs) {
    New-ADOrganizationalUnit -Name $ou -Path "OU=Byron_Corp,DC=byronlab,DC=local"
}
New-ADOrganizationalUnit -Name "Engineering" -Path "OU=Users,OU=Byron_Corp,DC=byronlab,DC=local"

# Aprovisionamiento de Cuenta de Usuario con Restablecimiento Obligatorio
$password = ConvertTo-SecureString "Password123!" -AsPlainText -Force
New-ADUser -Name "John White" -SamAccountName "jwhite" `
           -UserPrincipalName "jwhite@byronlab.local" `
           -Path "OU=Engineering,OU=Users,OU=Byron_Corp,DC=byronlab,DC=local" `
           -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $true
</pre>

        <h3>3. Endurecimiento de Seguridad vía GPO (Workstation Hardening)</h3>
        <p>Con el fin de mitigar vectores de ataques locales e impedir alteraciones arbitrarias en las estaciones de trabajo por parte de los usuarios de la sub-OU <code>Engineering</code>, diseñé la directiva vinculada <code>GPO_Engineering_Restrictions</code>:</p>
        <ul class="step-list">
            <li><strong>Restricción del Panel de Control:</strong> Habilitación de la directiva <i>"Prohibit access to Control Panel and PC settings"</i> para centralizar la gestión de adaptadores de red y configuraciones críticas de seguridad.</li>
            <li><strong>Mitigación de Ejecución de Comandos:</strong> Activación de la directiva <i>"Prevent access to the command prompt"</i>, bloqueando la consola interactiva (CMD) de forma inmediata al iniciar sesión para prevenir ejecuciones de scripts no autorizados por el usuario final.</li>
        </ul>

        <h3>4. Demostración en Video y Pruebas Operacionales</h3>
        <p>A continuación se presenta la verificación técnica interactiva del laboratorio, validando el proceso completo de inicio de sesión de usuario de dominio (<code>jwhite</code>) en Windows 11, la solicitud forzada del cambio de contraseña inicial y el bloqueo de los componentes restringidos por GPO:</p>

        <div class="video-container">
            <video width="100%" height="auto" controls poster="imagen_de_espera.png">
                <source src="videos/active-directory-demo.mp4" type="video/mp4">
                Tu navegador no soporta la reproducción de videos nativos en HTML5.
            </video>
        </div>
    </div>

    <div class="project-card">
        <h2>Proyecto: Laboratorio de Monitoreo NOC con Zabbix</h2>
        <p>Implementación de un entorno de monitoreo centralizado utilizando Zabbix sobre servidores virtuales Linux para supervisar disponibilidad, métricas de latencia por ICMP, rendimiento de CPU y gestión de alertas operacionales mediante dashboards en tiempo real.</p>
        <div>
            <span class="tech-tag">Zabbix</span>
            <span class="tech-tag">Linux (Ubuntu Server)</span>
            <span class="tech-tag">NOC Dashboards</span>
            <span class="tech-tag">ICMP & SNMP Monitoring</span>
        </div>
    </div>

</body>
</html>
