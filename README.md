# rxg_weapon_list_planning

I'm not organized but this is what I've thought of so far.


## Questions ##

* What kind of database server are we using? MySQL, postgres, something crazy? Is there something usable on the server already?
* When the tf2items.weapons.txt file is updated does the server instantly used the updated stats? Or does something need to happen before they are used?
* How are we going to handle the addition of new weapons / stats?
 * This could be done manually through the backend interface. So we would have a create form for weapons and one for stats and admins could do this themselves.
 * We could attempt to periodically check for new weapons / stats and add them automatically.
  * The Steam web api allows us to get the current [Item schema](https://wiki.teamfortress.com/wiki/Item_schema) which is a json file containing...pretty much everything we could ever want  (all items, attributes, and other stuff). You can access the tf2 item schema here "http://api.steampowered.com/IEconItems_440/GetSchema/v0001?language=en_US&key=XXXXXXXXXXXXXXXXXXXXXXX" (You can get a key [here](http://steamcommunity.com/dev/registerkey)). So we could periodically hit that url, get the schema, check for any new weapons/attributes, add them to the database and.... email an admin so they can make it super OP.
   * Note: You should specify the language param in the http request. This causes the items names to use english as opposed to tokens (ex. "Kukri", vs "#TF_Weapon_Club")
  

## Possible DB Schema ##


### Person ###
* id
* friendly_name
  * example: "Roker"
* tf2items_id 
  * example: "STEAM_0:1:13776935" or "*"

### weapon ###
Note: For this one I copied anything I thought might be remotely useful from the item schema

* name
* defindex
* item_class
* item_type_name
* item_name
* item_description
* proper_name
* item_slot
* item_quality
* image_url
* image_url_large
* min_ilevel
* max_ilevel

### attribute ###
Note: this structure is taken directly from the item schema
* name
* defindex
* attribute_class
* description_string
* description_format
* effect_type
* hidden
* stored_as_integer

### class ###
* id
* name

### weapon_classes ###
* weapon_id
* class_id

### weapon_attributes ###
* weapon_id
* attribute_id
* stat_value
* created_at
* updated_at

### push_log ###
* id
* pushed_at


## Example tf2items.weapons.txt file ##




## Front end technologies ##

* [angular](https://angularjs.org/)
* [angular-seed](https://github.com/angular/angular-seed)

## Backend Technologies ##
* PHP 5.4+
* [Laravel](http://laravel.com/)
* Some kind of database

## Useful Links ##

* [Find your current steam web api key](https://steamcommunity.com/dev/apikey)
* [Description of the GetSchema call](https://wiki.teamfortress.com/wiki/WebAPI/GetSchema)
* [Example tf2items.weapons.txt](http://hg.limetech.org/projects/tf2items/tf2items_source/diff/19eeebf8ccaa/tf2items.weapons.txt)
* [Parse VDF to JSON]https://gist.github.com/AlienHoboken/5571903

## High level functionality ##

* Abilty for admins to edit what stats are displayed on the weaponlist front end with a nice interface
 * Example: admin goes to single weapon page and can add/remove/edit the stats for that weapon
* Ability to import the data in tf2items.weapons.txt 
 * Question: Is there only one of these or do all x10 servers have the same one? If they are different are we going to want to be able import data for all of them or just one? or....
* Ability to generate a tf2items.weapons.txt file from the database and possibly automatically push the file to the x10 servers.
 * goal here is eliminate the need to manually edit the tf2items.weapson.txt file
