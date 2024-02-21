# EVERYTHING IS WORK IN PROGRESS


# Config API
> A Config API for Forge, NeoForge, Fabric and Quilt.

---
### How to add Config API to my project?
1. Open your build.gradle
2. Now you need to add a plugin to your gradle project
   - ```
     plugins {
        ...
        id 'io.github.0ffz.github-packages' version '1.2.+'
     }
     ```
3. The next step is, to add inside the repositories the following maven repository
    - ```
      repositories {
        ... 
        maven githubPackage.invoke("Craftjakob/Config-API")
      }
4. Now you can add one of these dependencies to your project. Make sure, that you add the dependency, that you need. So if you use forge, then use the forge dependency.
    - if you can not use modImplementation, then use compileOnly and runtimeOnly.
    - ```
      dependencies {
        ...
        modImplementation "com.craftjakob:configapi-common:${mc_version}-${configapi_version}"
        modImplementation "com.craftjakob:configapi-fabric:${mc_version}-${configapi_version}"
        modImplementation "com.craftjakob:configapi-forge:${mc_version}-${configapi_version}"
        modImplementation "com.craftjakob:configapi-neoforge:${mc_version}-${configapi_version}"
        modImplementation "com.craftjakob:configapi-quilt:${mc_version}-${configapi_version}"
     }
5. Finally, you can reload your gradlew project
---
### How to use it?

1. Create a new class, which contains your configs
2. The Config Class need to implement 'IConfigurator' and then implement the 'configure' methode
    - ```
      public class ExampleConfig implements IConfigurator {
         @Override
         public void configure(ConfigBuilder builder) {
        
         }
      }
3. Create a public static field with the ConfigValue, that you want
    - ```
      public static ConfigValueTypes.BooleanValue ExampleBooleanConfig;
4. Inside the 'configure' methode, you need to create the config
    - ```
      @Override
      public void configure(ConfigBuilder builder) {
        ExampleBooleanConfig = builder.define("ExampleBooleanConfig", true);
      }
5. Register your Config Class with ConfigRegister in your Main class. You can choose the config type between CLIENT, COMMON and SERVER.
    - ```
      public class ExampleMod {
         ...
         public static void init() {
             ConfigRegister.get().registerConfig(ConfigRegister.ConfigType.COMMON, ExampleConfig::new, MOD_ID);
             ...   
         }
         ...
      }
    - You can also give your config file a specific name:
    - ```
      ConfigRegister.get().registerConfig(ConfigRegister.ConfigType.COMMON, ExampleConfig::new, MOD_ID, "CUSTOM_NAME");
---
### ConfigTypes
> Client
>
> > Client is loaded, when the client setup, of the specific mod loader is loaded. On server the file gots not created.
>
> Common
>
> > The Common one, is the safest to use, it loads directly and does not require something, that need to be started. It loads directly in registrering.
>
> Server
>
> > The Server type is only loaded, if the server is started. The config file is only created, if you want to open the config file in the Config Screen, but it is not tracked.
---
#### Optional Config Settings

> Inside the ConfigBuilder are different methods to use
1. One methode is the 'comment' methode, it creates a comment for the specific config or category
    - ```
      public class ExampleConfig implements IConfigurator {
         public static ConfigValueTypes.BooleanValue ExampleBooleanConfig;
         @Override
         public void configure(ConfigBuilder builder) {
             ExampleBooleanConfig = builder
                     .comment("This is an example comment")
                     .define("ExampleBooleanConfig", true);
         }
      }
2. In your Config Class you can use 'translation' to translate the comment into a different language, this is displayed in the Config Screen.
    - ```
      public class ExampleConfig implements IConfigurator {
         public static ConfigValueTypes.BooleanValue ExampleBooleanConfig;
         @Override
         public void configure(ConfigBuilder builder) {
             ExampleBooleanConfig = builder
                     .comment("This is an example comment")
                     .translation("modid.exampleTranslationName.somethingelse")
                     .define("ExampleBooleanConfig", true);
         }
      }
3. You can also use 'requiresWorldRestart', it is only a comment wich says, that this config needs a restart.
    - ```
      @Override
      public void configure(ConfigBuilder builder) {
          ExampleBooleanConfig = builder
              .comment("This is an example comment")
              .translation("modid.exampleTranslationName.somethingelse")
              .requiresWorldRestart()
              .define("ExampleBooleanConfig", true);
      }
4. There is also a 'requiresClientRestart' method, that you can use.
---

### Give your config a category
> If you want, that your config file is more ordered, then use push(...) and pop()
- 'push(...)' requires a String, it is the category name
- 'pop()' pushes the category to the right
- 'pop(count)' allows you to move the category to the right as many times as you want
- Example:
    - ```
      @Override
      public void configure(ConfigBuilder builder) {
        builder.push("First Category");
            ExampleBooleanConfig = builder.define("ExampleBooleanConfig", true);
        builder.pop()
        ExampleIntegerConfig = builder.define("ExampleIntegerConfig", 5, 0, 10);
      } 
    - ```
      [First Category]
        ExampleBooleanConfig = true
      
      #Range: 0 ~ 10
      ExampleIntegerConfig = 5
- You also can give your category a comment or a translation key:
- ```
  @Override
    public void configure(ConfigBuilder builder) {
        builder.comment("A Comment for a Category!").translation("config.examplemod.category).push("First Category");
            ExampleBooleanConfig = builder.define("ExampleBooleanConfig", true);
        builder.pop()
    } 
---

### Get different values
> You have different values, that you can get from your config.
> To get these values, use 'MyConfigClass.MyConfig'
- getValue() -> gets the current config value
- getDefaultValue() -> gets the default value
- getRange() gets the range (Only for values like Integers, Floats, etc.)
    - if you call the getRange() methode, you can call the following methods:
        - getMinValue() -> gets the minimum value, that is allowed for you config
        - getMaxValue() -> gets the maximum value, that is allowed for you config
        - toString() -> gives a String in this format: "minValue ~ maxValue"
- getComments() -> gets all comments in a List of Strings
- getComment() -> all comments are seperated by a new line
- getPath() -> gets the path, in which the config is + config key
- getKey() -> gets the Key (Config Name)
    - ```
      ExampleBooleanConfig = builder.define("ExampleBooleanConfig", true);
    - It is the String, that you need to define
- getTranslationKey() -> gets the translation key
- getRequiresWorldRestart() -> gets the value (true or false) for requiresWorldRestart
- getRequiresClientRestart() -> it's the same as getRequiresWorldRestart(), but only for client
- getParser() -> returns the parser
- getBuilder() -> gets the ConfigBuilder
- getConfig() -> gets the Config class, so you can load or save your config

### Possible Values to configure
> These all values can be configured via this Config API
> - Booleans
> - Characters
> - Strings
> - Enums
> - Lists
> - Bytes
> - Shorts
> - Integers
> - Longs
> - Floats
> - Doubles

> You can also create custom ones, by using ConfigValue or for Numbers NumberValue
- ```
  public class ExampleConfig implements IConfigurator {
      public static ConfigValueTypes.ConfigValue<MyCustomValue> MyCustomValueConfig;
      public static ConfigValueTypes.ConfigValue<MyCustomNumberValue> MyCustomNumberValueConfig;
      @Override
      public void configure(ConfigBuilder builder) {
          MyCustomValueConfig = builder.define("MyCustomValueConfig", DEFAULT_VALUE, MY_PARSER);
          MyCustomNumberValueConfig = builder.defineInRange("MyCustomNumberValueConfig", DEFAULT_VALUE, MIN_VALUE, MAX_VALUE, MY_PARSER);
      }
  }
### Config Screen
> The Config Screen is automatically added to your project.
> - In Forge and NeoForge, it's in the mods screen, there you can click on 'Config' to open the screen
> - Fabric and Quilt has currently no Config Screen, but it will have one maybe via the 'Mod Menu' Mod
