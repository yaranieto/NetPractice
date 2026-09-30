# NetPractice

*Este proyecto ha sido creado como parte del currículum de 42 por ynieto-s*

## Descripción

NetPractice es un ejercicio práctico sobre fundamentos de redes que consiste en 10 niveles de dificultad creciente. El objetivo es configurar correctamente direcciones IP, máscaras de subred y tablas de enrutamiento para hacer funcionar diagramas de red.

Cada nivel presenta una topología de red diferente que requiere configuración adecuada para permitir la comunicación entre todos los dispositivos.

## Instrucciones

### Ejecutar la Interfaz

Abrir el archivo `index.html` en el navegador web. Seleccionar un nivel (1-10) para comenzar a practicar.

### Completar Niveles

1. Analizar la topología de red presentada
2. Configurar direcciones IP y máscaras de subred para cada interfaz
3. Configurar tablas de enrutamiento donde sea necesario
4. Verificar la configuración con "Check again"
5. Exportar el archivo de configuración con el botón "Export"

### Requisitos de Entrega

El repositorio debe contener:
- Este archivo README.md
- 10 archivos de configuración: `level1.json` hasta `level10.json`
- Todos los archivos en la raíz del repositorio

## Conceptos de Redes

### 📡 Direccionamiento TCP/IP

```
Estructura de dirección IP:
192.168.1.10
│   │   │ │
│   │   │ └─ Host
│   │   └─── Subred
│   └─────── Red privada
└─────────── Clase C
```

**Direcciones IP Privadas:**
- `10.0.0.0/8` → 10.0.0.0 - 10.255.255.255
- `172.16.0.0/12` → 172.16.0.0 - 172.31.255.255
- `192.168.0.0/16` → 192.168.0.0 - 192.168.255.255

### 🎭 Máscaras de Subred y CIDR

```
Ejemplo Visual:

IP:      192.168.1.10  →  11000000.10101000.00000001.00001010
Máscara: 255.255.255.0 →  11111111.11111111.11111111.00000000
                          \_________RED_________/\____HOST____/

Notación CIDR: 192.168.1.10/24
```

**Tabla de Conversión CIDR:**

| CIDR | Máscara | Hosts | Uso |
|------|---------|-------|-----|
| /30 | 255.255.255.252 | 2 | Enlace punto a punto |
| /29 | 255.255.255.248 | 6 | Subred muy pequeña |
| /28 | 255.255.255.240 | 14 | Subred pequeña |
| /27 | 255.255.255.224 | 30 | Subred pequeña-mediana |
| /26 | 255.255.255.192 | 62 | Subred mediana |
| /24 | 255.255.255.0 | 254 | Red clase C completa |

### 🔢 Cálculo de Subredes

**Ejemplo: Dividir 192.168.1.0/24 en 4 subredes**

```
Nueva máscara: /26 (255.255.255.192)

Subred 1: 192.168.1.0/26
├─ Red:              192.168.1.0
├─ Primera IP:       192.168.1.1
├─ Última IP:        192.168.1.62
└─ Broadcast:        192.168.1.63

Subred 2: 192.168.1.64/26
Subred 3: 192.168.1.128/26
Subred 4: 192.168.1.192/26
```

### 🚪 Puerta de Enlace (Gateway)

```
  Red A: 192.168.1.0/24              Red B: 10.0.0.0/24
  ┌─────────────────┐                ┌─────────────────┐
  │  PC1            │                │            PC2  │
  │  192.168.1.10   │                │  10.0.0.10      │
  │  GW: 192.168.1.1│                │  GW: 10.0.0.1   │
  └────────┬────────┘                └────────┬────────┘
           │                                  │
      ┌────┴──────────────────────────────────┴────┐
      │         ROUTER                             │
      │  Interface A: 192.168.1.1                  │
      │  Interface B: 10.0.0.1                     │
      └────────────────────────────────────────────┘
```

### 🗺️ Tablas de Enrutamiento

```
Ejemplo de Tabla:

┌────────────────────┬──────────────┬────────────────┐
│ Destino            │ Gateway      │ Interfaz       │
├────────────────────┼──────────────┼────────────────┤
│ 0.0.0.0/0          │ 192.168.1.1  │ eth0           │ ← Ruta predeterminada
│ 192.168.1.0/24     │ 0.0.0.0      │ eth0           │ ← Red local (directa)
│ 10.0.0.0/24        │ 192.168.1.254│ eth0           │ ← Red remota
└────────────────────┴──────────────┴────────────────┘

0.0.0.0/0: Cualquier destino no especificado
0.0.0.0 como gateway: Conexión directa (misma red)
```

### 🔄 Routers vs Switches

```
SWITCH (Capa 2)              ROUTER (Capa 3)
┌─────────────────┐          ┌─────────────────┐
│    [Switch]     │          │    [Router]     │
│   │  │  │  │    │          │   /        \    │
│  PC1 PC2 PC3 PC4│          │ Red A    Red B  │
│                 │          │                 │
│ - Direcciones MAC│          │ - Direcciones IP│
│ - Misma red     │          │ - Conecta redes │
└─────────────────┘          └─────────────────┘
```

## Ejemplos Prácticos

### Ejemplo 1: Red Simple

```
┌──────────┐                    ┌──────────┐
│   PC1    │────────────────────│   PC2    │
└──────────┘                    └──────────┘

PC1: 192.168.1.1/30
PC2: 192.168.1.2/30

✅ Red: 192.168.1.0/30
✅ IPs usables: .1 y .2
✅ Broadcast: 192.168.1.3
```

### Ejemplo 2: Red con Router

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│   PC1    │────│  Router  │────│   PC2    │
└──────────┘    └──────────┘    └──────────┘

PC1:
  IP: 10.0.0.2/25
  Gateway: 10.0.0.1

Router:
  Interface A: 10.0.0.1/25
  Interface B: 10.0.0.129/25

PC2:
  IP: 10.0.0.130/25
  Gateway: 10.0.0.129

✅ Red A: 10.0.0.0/25 (IPs: .1 - .126)
✅ Red B: 10.0.0.128/25 (IPs: .129 - .254)
```

## Checklist de Verificación

```
□ ¿Las IPs están en el mismo rango de subred?
□ ¿La IP no es dirección de red ni broadcast?
□ ¿El gateway está en la misma subred?
□ ¿Existe ruta predeterminada (0.0.0.0/0) cuando sea necesario?
□ ¿Las interfaces del router tienen IPs correctas?
```

## Errores Comunes

```
❌ IPs en subredes diferentes sin router
❌ Usar dirección de red (.0) o broadcast (.255)
❌ Gateway inalcanzable (fuera de la subred)
❌ Máscara incorrecta (dispositivos en diferentes subredes)
```

## Calculadora Mental Rápida

```
Para /26 (255.255.255.192):
  256 - 192 = 64
  Subredes: 0, 64, 128, 192

Para /27 (255.255.255.224):
  256 - 224 = 32
  Subredes: 0, 32, 64, 96, 128, 160, 192, 224

Para /28 (255.255.255.240):
  256 - 240 = 16
  Subredes: 0, 16, 32, 48, 64, 80, 96...
```

## Progresión de Niveles

```
Level 1-2:  ████░░░░░░ Básico      → IPs y máscaras simples
Level 3-4:  █████░░░░░ Fácil       → Introducción a routers
Level 5-6:  ██████░░░░ Intermedio  → Tablas de enrutamiento
Level 7-8:  ███████��░░ Avanzado    → Múltiples routers
Level 9-10: ██████████ Experto     → Topologías complejas
```

## Recursos

### Documentación Oficial
- [RFC 791 - Internet Protocol](https://tools.ietf.org/html/rfc791) - Especificación IPv4
- [RFC 1918 - Private Addresses](https://tools.ietf.org/html/rfc1918) - Direcciones privadas

### Tutoriales
- [Cisco Networking Basics](https://www.cisco.com/c/en/us/solutions/small-business/resource-center/networking/networking-basics.html)
- [TCP/IP Guide](http://www.tcpipguide.com/)
- [IP Subnetting for New Users](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html)

### Herramientas
- [Subnet Calculator](https://www.subnet-calculator.com/)
- [Visual Subnet Calculator](https://www.davidc.net/sites/default/subnets/subnets.html)
- [Calculadora en Español](https://www.site24x7.com/es/tools/ipv4-subred-calculadora.html)

### Uso de IA

Se utilizó asistencia de IA para:
- Aclaración de conceptos complejos de networking
- Investigación y resumen de documentación RFC
- Depuración de errores de configuración
- Creación de este README con esquemas visuales

**Nota**: Las configuraciones de los 10 niveles se completaron manualmente. La IA se utilizó únicamente como herramienta de aprendizaje y documentación.

## Estructura del Proyecto

```
netpractice/
├── README.md
├── level1.json
├── level2.json
├── level3.json
├── level4.json
├── level5.json
├── level6.json
├── level7.json
├── level8.json
├── level9.json
└── level10.json
```

---

**¡Buena suerte!** 🌐
