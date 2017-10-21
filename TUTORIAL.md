# Tutorial
Ionic-Electron Template

Last Update: 21. October 2017

## How to create this template?

1. Open the folder where the project should be created and run the command below. If you are in folder 'c:\projects\' the folder 'c:\projects\ionic-electron' will be created with all necessary files of the ionic project.
  ```bash
  $ ionic start ionic-electron blank
  ```
2. Open the folder, which you created the step before and run the command below. If everything was installed successfully a web browser will be open and show you the Ionic blank page of the project.
  ```bash
  $ ionic serve
  ```
3. Install the packages 'electron' to the file '/package.json':
  ```bash
  $ npm install electron@1.7.9 --save
  ```
4. Create the folder '/config/':
  ```bash
  /config/
  ```
5. Add the file '/config/electron.copy.js' to the folder '/config/':
  ```js
  module.exports = {
      copyAssets: {
          src: ['{{SRC}}/assets/**/*'],
          dest: '{{WWW}}/assets'
      },
      copyIndexContent: {
          src: ['{{SRC}}/index.html', '{{SRC}}/manifest.json'],
          dest: '{{WWW}}'
      },
      copyFonts: {
          src: ['{{ROOT}}/node_modules/ionicons/dist/fonts/**/*', '{{ROOT}}/node_modules/ionic-angular/fonts/**/*'],
          dest: '{{WWW}}/assets/fonts'
      },
      copyPolyfills: {
          src: ['{{ROOT}}/node_modules/ionic-angular/polyfills/polyfills.js'],
          dest: '{{BUILD}}'
      },
      copySwToolbox: {
          src: ['{{ROOT}}/node_modules/sw-toolbox/sw-toolbox.js'],
          dest: '{{BUILD}}'
      }
  };
  ```
6. Create the file '/main.js' and add the following code:
  ```js
  const electron = require('electron');
  // Module to control application life.
  const app = electron.app;
  // Module to create native browser window.
  const BrowserWindow = electron.BrowserWindow;
  
  const Menu = electron.Menu;
  
  const Tray = electron.Tray;
  
  
  const path = require('path');
  const url = require('url');
  
  // Keep a global reference of the window object, if you don't, the window will
  // be closed automatically when the JavaScript object is garbage collected.
  var mainWindow;
  var trayApp;
  
  function createWindow () {
      // Create the browser window.
      mainWindow = new BrowserWindow({
          width: 800,
          height: 600,
          title: 'Your title',
          icon: 'electron/icon.png'
      });
  
      // and load the index.html of the app.
      mainWindow.loadURL(url.format({
          pathname: path.join(__dirname, "www/index.html"),
          protocol: "file:",
          slashes: true
      }));
  
      buildTrayIcon();
  
      // Open the DevTools.
      // mainWindow.webContents.openDevTools();
  
      // Emitted when the window is closed.
      mainWindow.on("closed", function () {
          // Dereference the window object, usually you would store windows
          // in an array if your app supports multi windows, this is the time
          // when you should delete the corresponding element.
          mainWindow = null
      });
  }
  
  // This method will be called when Electron has finished
  // initialization and is ready to create browser windows.
  // Some APIs can only be used after this event occurs.
  app.on("ready", createWindow);
  
  // Quit when all windows are closed.
  app.on("window-all-closed", function () {
      // On OS X it is common for applications and their menu bar
      // to stay active until the user quits explicitly with Cmd + Q
      if (process.platform !== "darwin") {
          app.quit();
      }
  });
  
  app.on("activate", function () {
      // On OS X it's common to re-create a window in the app when the
      // dock icon is clicked and there are no other windows open.
      if (mainWindow === null) {
          createWindow();
      }
  });
  
  // In this file you can include the rest of your app's specific main process
  // code. You can also put them in separate files and require them here.
  
  function buildTrayIcon () {
      var trayIconPath = path.join(__dirname, 'electron/icon.png');
      var contextMenu = Menu.buildFromTemplate([
          {
              label: 'App anzeigen',
              click:  function(){
                  mainWindow.show();
              }
          },
          {
              label: 'Schließen',
              click:  function(){
                  app.isQuiting = true;
                  app.quit();
              }
          }
      ]);
  
      trayApp = new Tray(trayIconPath);
      trayApp.setToolTip('Your app');
      trayApp.setContextMenu(contextMenu);
  }
  ```
7. Create the folder '/electron/' and add an image 'icon.png' with a size of 18x18:
8. Add the following code to the file '/package.json':
  ```json
  "main": "main.js",
  "scripts": {
    "electron": "ionic-app-scripts build --copy ./config/electron.copy.js",
    "electron-start": "electron ."
  }
  ```
9. Build the project:
  ```bash
  $ npm run build
  ```
10. Run the project as an electron app:
  ```bash
  $ npm run electron-start
  ```
11. Install the packages 'electron-packager' to the file '/package.json':
  ```bash
  $ npm install electron-packager@9.1.0 --save
  ```
12. Add the following code to the file '/package.json':
  ```json
  "scripts": {
    ...
    "electron-build": "ionic-app-scripts build && electron-packager . YourApp --platform=win32 --arch=x64 --out=./dist/",
    ...
  }
  ```
13. Build the project as an electron app:
  ```bash
  $ npm run electron-build
  ```
