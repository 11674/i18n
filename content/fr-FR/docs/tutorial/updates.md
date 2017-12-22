# Mettre à jour l'application

Il y a plusieurs méthodes pour mettre à jour une application Electron. La plus simple et officielle profite de l'intégration du framework [Squirrel](https://github.com/Squirrel) et du module [autoUpdater](../api/auto-updater.md) d'Electron.

## Déploiement d’un serveur de mise à jour

Pour commencer, vous devez d'abord déployer un serveur dont le module [autoUpdater](../api/auto-updater.md) ira télécharger les nouvelles mises à jour.

Selon vos besoins, vous pouvez choisir parmi l'un d'entre eux :

- [Hazel](https://github.com/zeit/hazel) – Update server for private or open-source apps. Can be deployed for free on [Now](https://zeit.co/now) (using a single command), pulls from [GitHub Releases](https://help.github.com/articles/creating-releases/) and leverages the power of GitHub's CDN.
- [Nuts](https://github.com/GitbookIO/nuts) - Utilise également les [versions GitHub](https://help.github.com/articles/creating-releases/), mais met en cache les mises à jour sur le disque et prend en charge les dépôts privés.
- [electron-release-server](https://github.com/ArekSredzki/electron-release-server) – Provides a dashboard for handling releases
- [Nucleus](https://github.com/atlassian/nucleus) – A complete update server for Electron apps maintained by Atlassian. Prend en charge plusieurs applications et canaux; utilise un magasin de fichier statiques pour rapetisser le coût serveur.

Si votre application est empaquetée avec [electron-builder](https://github.com/electron-userland/electron-builder), vous pouvez utiliser le module [electron-updater](https://www.electron.build/auto-update) qui ne nécessite pas de serveur et permet les mises à jour depuis S3, GitHub ou tout autre hôte de fichiers statiques.

## Implémentation des mises à jour dans votre application

Une fois que vous avez déployé votre serveur de mise à jour, continuez à importer les modules requis dans votre code. Le code suivant peut varier pour les différents serveurs, mais il fonctionne comme décrit lors de l'utilisation de [Hazel](https://github.com/zeit/hazel).

**Important :** Veuillez vous assurer que le code ci-dessous sera exécuté uniquement dans votre application empaqueté et non en développement. Vous pouvez utiliser [electron-is-dev](https://github.com/sindresorhus/electron-is-dev) pour vérifier l'environnement.

```js
const {app, autoUpdater, dialog} = require('electron')
```

Ensuite, construisez l'URL du serveur de mise à jour et informez-en [autoUpdater](../api/auto-updater.md):

```js
const server = 'https://your-deployment-url.com'
const feed = `${server}/update/${process.platform}/${app.getVersion()}`

autoUpdater.setFeedURL(feed)
```

Comme dernière étape, vérifiez les mises à jour. L'exemple ci-dessous vérifie chaque minute:

```js
setInterval(() => {
  autoUpdater.checkForUpdates()
}, 60000)
```

Une fois que votre application est [empaqueté](../tutorial/application-distribution.md), il recevra une mise à jour pour chaque nouvelle [version GitHub](https://help.github.com/articles/creating-releases/) que vous publierez.

## Appliquer la mise à jour

Maintenant que vous avez configuré le mécanisme de mise à jour de base de votre application, vous devez vous assurer que l’utilisateur sera notifié quand il y a une mise à jour. Cela peut être fait en utilisant l'API [events](../api/auto-updater.md#events) d'autoUpdater :

```js
autoUpdater.on('update-downloaded', (event, releaseNotes, releaseName) => {
  const dialogOpts = {
    type: 'info',
    buttons: ['Restart', 'Later'],
    title: 'Application Update',
    message: process.platform === 'win32' ? releaseNotes : releaseName,
    detail: 'A new version has been downloaded. Restart the application to apply the updates.'
  }

  dialog.showMessageBox(dialogOpts, (response) => {
    if (response === 0) autoUpdater.quitAndInstall()
  })
})
```

Veillez également à ce que les erreurs sont [traitées](../api/auto-updater.md#event-error). Voici un exemple de leur affichage dans `stderr` :

```js
autoUpdater.on('error', message => {
  console.error('There was a problem updating the application')
  console.error(message)
})
```