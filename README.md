### Configuraciones Previas

1. Creo proyecto con NextJS

```
npx create-next-app frontend
```

2. Configuro el archivo package.json para que el servidor se ejecute en el puerto 3001

```
"scripts": {
  "dev": "next dev -p 3001",
...
}
```

3. **EL BACKEND Y POSTGRESQL DEBEN SEGUIR EJECUTANDOSE**

```
npm run start
```

---

4. Instalar Tailwind CSS

```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

5. Configurar tailwind.config.js

```
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}", // Asegúrate de que esta línea esté presente
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

6. Importar Tailwind CSS en globals.css

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

7. Importar global.css en archivo layout.tsx

```
import "./globals.css";
```

8. Creo formulario con estilos Tailwind CSS en archivo principal page.tsx

```
"use client";

import { useState } from "react";

export default function Home() {
  const [formData, setFormData] = useState({ name: "", email: "" });
  const [alertVisible, setAlertVisible] = useState(false); // Estado para manejar la visibilidad de la alerta

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData({ ...formData, [name]: value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const response = await fetch('http://localhost:3000/form', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData),
    });

    const data = await response.json();
    console.log(data);

    // Mostrar la alerta
    setAlertVisible(true);

    // Ocultar la alerta después de 3 segundos
    setTimeout(() => {
      setAlertVisible(false);
    }, 3000);
  };

  return (
    <div className="flex items-center justify-center min-h-screen bg-gray-100">
      <form onSubmit={handleSubmit} className="bg-white p-6 rounded-lg shadow-md w-96">
        <h2 className="text-2xl font-bold mb-4 text-center">Formulario</h2>

        {/* Alerta de éxito */}
        {alertVisible && (
          <div className="mb-4 p-4 text-green-700 bg-green-100 rounded" role="alert">
            Datos enviados correctamente.
          </div>
        )}

        <div className="mb-4">
          <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="name">
            Nombre
          </label>
          <input
            type="text"
            name="name"
            placeholder="Nombre"
            value={formData.name}
            onChange={handleChange}
            required
            className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          />
        </div>
        <div className="mb-4">
          <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="email">
            Email
          </label>
          <input
            type="email"
            name="email"
            placeholder="Email"
            value={formData.email}
            onChange={handleChange}
            required
            className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          />
        </div>
        <button
          type="submit"
          className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline w-full"
        >
          Enviar
        </button>
      </form>
    </div>
  );
}
```

9. Ejecuto App

```
npm run dev
```
