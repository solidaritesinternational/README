# Solis Coding Practices 


## Solis Ecosystem

- Solis > solis2
- Solis referral > solis
- Solis Bot > solbot2


## Database

Every Solis app use PostgreSQL
Some systems may use multiple databases. (local and remote)

## Comments

All functions should be commented as follows :

```php
/**
 * Summary; a one-liner which globally states the function of the documented element. 
 * 
 * @author [name] [<email address>]
 * @todo [description]
 * @deprecated [<Semantic Version>] [<description>]
 * @param [<Type>] [name] [<description>]
 * @return [Type] [<description>]
 */
```

The objective is twofold. To be able to use automatic documentation generators and to allow to maintain the code and to question its author if needed.

## Tests

For each new method, a unit test should be written.
The goal is to have 100% test coverage

## Git

Each new feature must be developed on a dedicated branch. When the feature is ready, create a Pull Request.

## Versionning

When the master branch receives updates a new release with the version number must be created. The added or updated features should be detailed in the release description.

### Version number convention

`[major].[minor].[patch]-[build/beta/rc]`

> Ex: v1.2.5

- Major versions contain breaking changes.
- Minor versions add new features or deprecate existing features without breaking changes.
- Patch versions fix defects or optimize existing features without breaking changes.
