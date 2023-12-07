# Config API
> A Config API for Forge, NeoForge, Fabric and Quilt.

---
### How to use it?

1. Create a new class, which contains your configs
2. The Config Class need to implement 'IConfigRegisterer' and then implement the 'configure' methode
   - ```
     public class ExampleConfig implements IConfigRegisterer {
        @Override
        public void configure(ConfigBuilder builder) {
        
        }
     }
3. Create a public static field with the ConfigValue, that you want
   - ```
     public static BooleanValue ExampleBooleanConfig;
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
            ConfigRegister.registerConfig(ConfigRegister.ConfigType.COMMON, ExampleConfig::new, MOD_ID);
            ...   
        }
        ...
     }
---
#### Optional Config Settings

> Inside the ConfigBuilder are different methods to use
1. One methode is the 'comment' methode, it creates a comment inside the config file
   - ```
     public class ExampleConfig implements IConfigRegisterer {
        public static BooleanValue ExampleBooleanConfig;
        @Override
        public void configure(ConfigBuilder builder) {
            ExampleBooleanConfig = builder
                    .comment("This is an example comment")
                    .define("ExampleBooleanConfig", true);
        }
     }
2. In your Config Class you can use 'translation' to translate the comment into a different language.
   - ```
     public class ExampleConfig implements IConfigRegisterer {
        public static BooleanValue ExampleBooleanConfig;
        @Override
        public void configure(ConfigBuilder builder) {
            ExampleBooleanConfig = builder
                    .comment("This is an example comment")
                    .translation("modid.exampleTranslationName.somethingelse")
                    .define("ExampleBooleanConfig", true);
        }
     }
3. You can also use 'requiresWorldRestart', it sets the new value in game, when you have restart the game
   - ```
     @Override
     public void configure(ConfigBuilder builder) {
         ExampleBooleanConfig = builder
             .comment("This is an example comment")
             .translation("modid.exampleTranslationName.somethingelse")
             .requiresWorldRestart()
             .define("ExampleBooleanConfig", true);
     }
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
      ExampleBooleanConfig=true
    
    #Range: 0 ~ 10
    ExampleIntegerConfig=5
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
- getComment() -> gets the comment
- getPath() -> gets the path, in which the config is
- getKey() -> gets the Key (Config Name)
  - ```
    ExampleBooleanConfig = builder.define("ExampleBooleanConfig", true);

  - It is the String, that you need to define
- getTranslationKey() -> gets the translation key
- getRequiresWorldRestart() -> gets the value (true or false) for requiresWorldRestart
     
