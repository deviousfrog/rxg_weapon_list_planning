# rxg_weapon_list_planning

I'm not organized but this is what I've thought of so far.


## Questions ##

* What kind of database server are we using? MySQL, postgres, something crazy? Is there something usable on the server already?
* When the tf2items.weapons.txt file is updated does the server instantly used the updated stats? Or does something need to happen before they are used?


## Possible DB Schema ##


### Person ###
* id
* friendly_name
  * example: "Roker"
* tf2items_id 
  * example: "STEAM_0:1:13776935" or "*"

### weapon ###
* tf2items_id (weapon ids from [here](https://wiki.alliedmods.net/Team_fortress_2_item_definition_indexes))
* friendly_name

### stat ###
* tf2items_id (ids from [here](https://wiki.teamfortress.com/wiki/List_of_item_attributes))
* description (description from same place [here](https://wiki.teamfortress.com/wiki/List_of_item_attributes))
* value_type (we might not need this for functionality but it might be useful to tell the admins what kind of number they are typing in, so this could be used in the UI. Again this is taken from [here](https://wiki.teamfortress.com/wiki/List_of_item_attributes))
* enabled (I'm assuming were going to somehow import all of the stats and some of them we will never want to use, so we could "disable" them and have them removed from the admin UI? to make things simpler?)

### class ###
* id
* friendly_name

### weapon_classes ###
* weapon_id
* class_id

### weapon_stats ###
* weapon_id
* stat_id
* stat_value
* created_at
* updated_at

### push_log ###
* id
* pushed_at


## High level functionality ##

* Abilty for admins ....ill do more later...
* Ability to generate a tf2items.weapons.txt file from the database.
