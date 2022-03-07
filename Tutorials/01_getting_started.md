# Getting Started

## Frontend

### 1. Download & Install Node.js

Go to https://nodejs.org/en/ to download 16.14.0.LTS

### 2. Download Git client

https://git-scm.com/downloads 

### 3. Install yarn

```bash
npm install --global yarn
```

(
    if you got an error with "cannot be loaded because running scripts is disabled on this system"
)

```bash
Set-ExecutionPolicy RemoteSigned
```

(run as administrator)

### 4. Install a umi-app/Ant design

```bash
mkdir frontend 
cd frontend
yarn create @umijs/umi-app
Yarn

npx umi g page posts --typescript
// Javascript
// npx umi g page posts

// in .umirc.ts configure
{ path: '/posts', component: '@/pages/posts' },


// Add a new file: src/components/PostList.tsx 
```
