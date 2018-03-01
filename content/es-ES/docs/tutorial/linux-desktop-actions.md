# Custom Linux Desktop Launcher Actions

En muchos ambientes Linux, puede agregar entradas personalizadas al lanzador modificando el archivo `.desktop`. Para la documentación de la unidad de Canonical, vea [Agregar accesos directos a un lanzador](https://help.ubuntu.com/community/UnityLaunchersAndDesktopFiles#Adding_shortcuts_to_a_launcher). Para obtener más información sobre una aplicación más genérica, vea la [especificación de freedesktop.org](https://specifications.freedesktop.org/desktop-entry-spec/1.1/ar01s11.html).

**Accesos directos del Launcher de Audacious:**

![audaz](https://help.ubuntu.com/community/UnityLaunchersAndDesktopFiles?action=AttachFile&do=get&target=shortcuts.png)

Generally speaking, shortcuts are added by providing a `Name` and `Exec` property for each entry in the shortcuts menu. Unity will execute the `Exec` field once clicked by the user. The format is as follows:

```text
Actions=PlayPause;Next;Previous

[Desktop Action PlayPause]
Name=Play-Pause
Exec=audacious -t
OnlyShowIn=Unity;

[Desktop Action Next]
Name=Next
Exec=audacious -f
OnlyShowIn=Unity;

[Desktop Action Previous]
Name=Previous
Exec=audacious -r
OnlyShowIn=Unity;
```

Unity's preferred way of telling your application what to do is to use parameters. You can find these in your app in the global variable `process.argv`.