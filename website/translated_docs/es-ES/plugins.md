---
id: plugins
title: "Plugins"
---

Verdaccio is an plugabble aplication. It can be extended in many ways, either new authentication methods, adding endpoints or using a custom storage.

There are 5 types of plugins:

* [Autenticación](plugin-auth.md)
* [Mediación](plugin-middleware.md)
* [Almacenamiento](plugin-storage.md)
* Custom Theme and filters

> If you are interested to develop your own plugin, read the [development](dev-plugins.md) section.

## Uso

### Instalación

```bash
$> npm install --global verdaccio-activedirectory
```

`verdaccio` as a sinopia fork it has backward compability with plugins that are compatible with `sinopia@1.4.0`. In such case the installation is the same.

    $> npm install --global sinopia-memory
    

### Configuración

Abra el archivo `config.yaml` y actualice la sección `auth` como a continuación:

La configuración por defecto luce así, debido a que usamos un plugin `htpasswd` incorporado por defecto que puede desactivar con solo comentar las siguientes líneas.

### Authentication Configuration

```yaml
 htpasswd:
    file: ./htpasswd
    # max_users: 1000
```

y reemplazándolos con (en caso de que decida usar un plugin `ldap`).

```yaml
auth:
  activedirectory:
    url: "ldap://10.0.100.1"
    baseDN: 'dc=sample,dc=local'
    domainSuffix: 'sample.local'
```

#### Multiple Authentication plugins

This is technically possible, making the plugin order important, as the credentials will be resolved in order.

```yaml
auth:
  htpasswd:
    file: ./htpasswd
    #max_users: 1000
  activedirectory:
    url: "ldap://10.0.100.1"
    baseDN: 'dc=sample,dc=local'
    domainSuffix: 'sample.local'
```

### Middleware Configuration

This is an example how to set up a middleware plugin. All middleware plugins must be defined in the **middlewares** namespace.

```yaml
middlewares:
  audit:
    enabled: true
```

> You might follow the [audit middle plugin](https://github.com/verdaccio/verdaccio-audit) as base example.

### Storage Configuration

This is an example how to set up a storage plugin. All storage plugins must be defined in the **store** namespace.

```yaml
store:
  memory:
    limit: 1000
```

### Theme Configuration

Verdaccio allows to replace the User Interface with a custom one, we call it **theme**. By default, uses `@verdaccio/ui-theme` that comes built-in, but, you can use something different installing your own plugin.

```bash
<br />$> npm install --global verdaccio-theme-dark

```

> The plugin name prefix must start with `verdaccio-theme`, otherwise the plugin won't load.

You can load only one theme at the time and pass through options if is need it.

```yaml
theme:
  dark:
    option1: foo
    option2: bar
```

## Plugins heredados

### Plugins de Sinopia

> If you are relying on any sinopia plugin, remember are deprecated and might no work in the future.

* [sinopia-npm](https://www.npmjs.com/package/sinopia-npm): plugin auth para sinopia soportando un registro npm.
* [sinopia-memory](https://www.npmjs.com/package/sinopia-memory): plugin auth para sinopia que mantiene a los usuarios en la memoria.
* [sinopia-github-oauth-cli](https://www.npmjs.com/package/sinopia-github-oauth-cli).
* [sinopia-crowd](https://www.npmjs.com/package/sinopia-crowd): plugin auth para sinopia que soporta atlassian crowd.
* [sinopia-activedirectory](https://www.npmjs.com/package/sinopia-activedirectory): plugin de autenticación Active Directory para sinopia.
* [sinopia-github-oauth](https://www.npmjs.com/package/sinopia-github-oauth): plugin de autenticación para sinopia2, el cual soporta el flujo web de github oauth.
* [sinopia-delegated-auth](https://www.npmjs.com/package/sinopia-delegated-auth): plugin de autenticación de Sinopia que delega autenticación a otro URL HTTP
* [sinopia-altldap](https://www.npmjs.com/package/sinopia-altldap): Alterna el plugin LDAP Auth para Sinopia
* [sinopia-request](https://www.npmjs.com/package/sinopia-request): Un plugin sencillo y completamente auth con configuración para usar una API externa.
* [sinopia-htaccess-gpg-email](https://www.npmjs.com/package/sinopia-htaccess-gpg-email): Genera contraseña en formato htaccess, encripta con GPG y la evía a través de la API MailGun a los usuarios.
* [sinopia-mongodb](https://www.npmjs.com/package/sinopia-mongodb): Un plugin fácil y completamente auth con configuración para usar una base de datos mongodb.
* [sinopia-htpasswd](https://www.npmjs.com/package/sinopia-htpasswd): plugin auth para sinopia que soporta el formato htpasswd.
* [sinopia-leveldb](https://www.npmjs.com/package/sinopia-leveldb): un plugin auth leveldb respaldado para el npm privado de sinopia.
* [sinopia-gitlabheres](https://www.npmjs.com/package/sinopia-gitlabheres): plugin de autenticación de Gitlab para sinopia.
* [sinopia-gitlab](https://www.npmjs.com/package/sinopia-gitlab): plugin de autenticación de Gitlab para sinopia
* [sinopia-ldap](https://www.npmjs.com/package/sinopia-ldap): plugin LDAP auth para sinopia.
* [sinopia-github-oauth-env](https://www.npmjs.com/package/sinopia-github-oauth-env) plugin de autenticación de Sinopia con flujo web github oauth.

> All sinopia plugins should be compatible with all future verdaccio versions. Anyhow, we encourage contributors to migrate them to the modern verdaccio API and using the prefix as *verdaccio-xx-name*.