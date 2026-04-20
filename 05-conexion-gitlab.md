# 05. Conexión con GitLab

Esta guía describe el proceso completo para conectar un proyecto local con GitLab, configurar acceso seguro por SSH e integrarlo con Jenkins.

---

## 1. Crear la cuenta en GitLab

1. Ingresar a https://gitlab.com
2. Crear una cuenta de usuario o iniciar sesión.

---

## 2. Configurar acceso por SSH

El acceso por SSH permite conectarse a GitLab sin usar usuario ni contraseña.

### 2.1 Crear la llave SSH en la VM

```bash
ssh-keygen -q -N "" -C "user@mail.com"
cat ~/.ssh/id_ed25519.pub
```

- Se genera un par de llaves:
  - **Privada**: se queda en la VM
  - **Pública**: se copia a GitLab

---

### 2.2 Agregar la llave SSH en GitLab

1. Clic en el **avatar del usuario**
2. Seleccionar **Edit profile / Preferencias**
3. Ir a **SSH Keys**
4. Pegar la llave pública
5. Asignar un **Title** (ejemplo: `demo`)
6. Guardar

---

## 3. Preparar el repositorio local

```bash
mkdir public-api
cd public-api
git config --global user.name "Administrator"
git config --global user.email "user@example.com"
git init --initial-branch=main
```

- Se crea el repositorio Git local
- La rama principal se llama `main`

---

## 4. Crear el proyecto en GitLab

1. Seleccionar **Create blank project**
2. Configurar:
   - **Project name**: public-api
   - **Visibility Level**: Private
   - **Initialize repository with a README**: ❌ false
3. Crear el proyecto

---

## 5. Integración GitLab con Jenkins

En el proyecto de GitLab:

1. Ir a **Settings → Integrations**
2. Seleccionar **Jenkins**
3. Configurar:
   - **Server URL** (ejemplo: http://34.73.192.17:8080)
   - **Project name**
   - **Username**
   - **Password / Token**
4. Probar con **Test settings**
5. Guardar con **Save**

---

## 6. Subir el código al proyecto GitLab

### 6.1 Clonar el proyecto base (desde GitHub)

```bash
git clone https://github.com/Storylabs-Learning/public-api-demo.git
mv public-api-demo/* .
rm -rf public-api-demo
ls
```

---

### 6.2 Asociar el repositorio local con GitLab (SSH)

```bash
git remote add origin git@gitlab.com:<USER>/public-api.git
```

Verificar:

```bash
git remote -v
```

---

### 6.3 Subir el código a GitLab

```bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

---

## 7. Resolución de errores comunes

### Error: `refusing to merge unrelated histories`

Este error ocurre porque el repositorio local y el remoto no comparten historial.

Solución:

```bash
git pull origin main --allow-unrelated-histories
```

Si aparecen conflictos:

1. Editar los archivos en conflicto
2. Eliminar las marcas `<<< === >>>`
3. Guardar cambios
4. Ejecutar:

```bash
git add .
git commit -m "Merge GitLab and local history"
git push origin main
```

---

## 8. Verificación final en GitLab

- Revisar que el código esté visible en el proyecto
- Confirmar que el commit fue subido correctamente
- Verificar ejecución de Jenkins (si está integrado)

---

## Recomendación académica

Para evitar conflictos en prácticas:

- Crear el proyecto **vacío** en GitLab
- Subir todo el código desde local
- No inicializar README si ya existe código
