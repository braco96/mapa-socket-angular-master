# Socket‑Server Multi Mapa

Servidor backend en **Node.js + Express + Socket.IO** (con TypeScript) que soporta visualización y gestión de ubicaciones/mapas en tiempo real.

> Proyecto basado en el curso de Fernando Herrera — “Sockets con mapas”.

---

## 🏗️ Características principales

- Autenticación mínima o asignación de usuario/socket.  
- Conexiones simultáneas por socket y gestión de estados de clientes.  
- Emisión de eventos por ubicación/mapa para visualización en tiempo real.  
- Rutas REST básicas + servidor HTTP + WebSocket integrados.  
- Código estructurado en carpetas: `classes/`, `routes/`, `sockets/`, `global/`.

---

## 🚀 Cómo ejecutar

```bash
git clone https://github.com/Klerith/socket-server-multi-mapa.git
cd socket-server-multi-mapa
npm install
npm run build   # o `tsc -w` para modo watch
npm start       # o `nodemon dist/` si tienes nodemon
```

Por defecto el servidor se levanta en el puerto definido en `global/environment.ts` (por defecto 5000).

---

## 📁 Estructura del proyecto

```
.
├── classes/          # Clases principales (Usuario, UsuariosLista, etc.)
├── global/
│   └── environment.ts  # Variables globales (SERVER_PORT, etc.)
├── routes/
│   └── router.ts       # Endpoints REST (por ejemplo /mapas, /usuarios, etc.)
├── sockets/
│   └── socket.ts       # Lógica de eventos Socket.IO (conectar, desconectar, mover, etc.)
├── index.ts           # Punto de entrada del servidor
├── tsconfig.json
└── package.json
```

---

## 🔌 Endpoints & Eventos

### REST

- `GET /usuarios` — retorna los IDs de los sockets conectados.  
- `GET /usuarios/detalle` — retorna detalles de los usuarios (id/nombre/estado).  
- `POST /movimiento` — (ejemplo) recibe un payload de ubicación para emitir a otros clientes.  
  *(Ajusta este endpoint según lo que el proyecto implemente en `routes/router.ts`.)*

### Socket.IO

- `connection` — cuando un cliente se conecta.  
- `desconectar` — cuando se desconecta; se emite `usuarios-activos`.  
- `configurar-usuario` — payload `{ nombre: string }`; callback con confirmación.  
- `ubicacion-nueva` — evento emitido con ubicación del usuario a todos los conectados.  
  *(Reemplaza `ubicacion-nueva` por el nombre de evento real que uses.)*

---

## 🎯 Uso típico del cliente frontend

1. Conectar al servidor de sockets (por ejemplo desde un cliente Angular, React o vanilla JS).  
2. Emitir `configurar-usuario` después de obtener nombre o credenciales del usuario.  
3. Escuchar `usuarios-activos` para actualizar lista de usuarios activos.  
4. Emitir `ubicacion-nueva` con latitud/longitud u otros datos del mapa.  
5. Escuchar ese mismo evento para actualizar la interfaz de mapa en tiempo real.

---

## 🔧 Ajustes y configuración

- Cambia el puerto editando `global/environment.ts`.  
- Asegúrate de que el cliente permita CORS (si tu frontend corre en un puerto distinto).  
- Si usas `nodemon`, agrega al `package.json` un script:

```json
"scripts": {
  "start": "node dist/",
  "dev": "nodemon dist/",
  "build": "tsc"
}
```

- Si deseas añadir autenticación o salas (rooms) en Socket.IO, puedes extender `sockets/socket.ts`.

---

## 📝 Licencia

MIT — Puedes usar y modificar este proyecto libremente.  
---

