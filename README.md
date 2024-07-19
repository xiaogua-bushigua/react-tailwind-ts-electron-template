### React-Vite-Tailwind CSS-Typescript-Electron-Template

- `create-vite`

- `npm install -D tailwindcss electron electron-builder postcss-cli autoprefixer`

- `npx tailwindcss init`

- tailwind.config.js

  ```javascript
  /** @type {import('tailwindcss').Config} */
  export default {
  	content: ['./src/**/*.{js,jsx,ts,tsx}'],
  	theme: {
  		extend: {},
  	},
  	plugins: [],
  };
  ```

- postcss.config.js

  ```javascript
  module.exports = {
  	plugins: [require('tailwindcss'), require('autoprefixer')],
  };
  ```

- main.js [root path]

  ```javascript
  const { app, BrowserWindow } = require('electron');
  const path = require('node:path');
  
  const createWindow = () => {
  	// Create the browser window.
  	const mainWindow = new BrowserWindow({
  		width: 800,
  		height: 600,
  		webPreferences: {
  			preload: path.join(__dirname, 'preload.js'),
        webSecurity: false
  		},
  	});
  
  	// Open the DevTools.
  	mainWindow.webContents.openDevTools();
  
  	// Log the file path to make sure it's correct
  	// console.log('Loading file:', path.join(__dirname, 'dist', 'index.html'));
  
  	// and load the index.html of the app.
  	mainWindow.loadFile(path.join(__dirname, 'dist', 'index.html')).catch((err) => {
  		console.error('Failed to load file:', err);
  	});
  };
  
  // This method will be called when Electron has finished
  // initialization and is ready to create browser windows.
  // Some APIs can only be used after this event occurs.
  app.whenReady().then(() => {
  	createWindow();
  
  	app.on('activate', () => {
  		// On macOS it's common to re-create a window in the app when the
  		// dock icon is clicked and there are no other windows open.
  		if (BrowserWindow.getAllWindows().length === 0) createWindow();
  	});
  });
  
  // Quit when all windows are closed, except on macOS. There, it's common
  // for applications and their menu bar to stay active until the user quits
  // explicitly with Cmd + Q.
  app.on('window-all-closed', () => {
  	if (process.platform !== 'darwin') app.quit();
  });
  
  // In this file you can include the rest of your app's specific main process
  // code. You can also put them in separate files and require them here.
  ```

- preload.js [root path]

  ```javascript
  const { contextBridge } = require('electron');
  
  contextBridge.exposeInMainWorld('versions', {
  	node: () => process.versions.node,
  	chrome: () => process.versions.chrome,
  	electron: () => process.versions.electron,
  	// we can also expose variables, not just functions
  });
  ```

- package.json

  ```json
  {
  	"name": "react-tailwind-ts-electron-template",
  	"private": true,
  	"version": "0.0.0",
  	"main": "main.js",
  	"build": {
  		"files": [
  			"dist/**/*",
  			"main.js",
  			"preload.js"
  		],
  		"directories": {
  			"buildResources": "build"
  		}
  	},
  	"scripts": {
  		"dev": "vite",
  		"build": "tsc && vite build",
  		"lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
  		"preview": "vite preview",
  		"start": "electron .",
  		"dist": "electron-builder"
  	},
  	"dependencies": {
  	},
  	"devDependencies": {
  	}
  }
  ```

- `npm run build` 
  - change `link rel="stylesheet" crossorigin href="/assets/index-CeJiBDz4.css">` into `link rel="stylesheet" crossorigin href="assets/index-CeJiBDz4.css">` (remove the first '/') in `/dist/index.html`
  - so does the script

- `npm run dist`
