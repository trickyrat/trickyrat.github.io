---
title: Redis Notes-04
date: 2022-01-18 01:08:20
categories: [Notes, Redis]
tags: Notes
---

## GeoHash Commands

### GEOADD

#### Syntax

> GEOADD key [NX | XX] [CH] longitude latitude member [longitude latitude member ...]  

Adds the specified geospatial items (longitude, latitude, name) to the specified key. Data is stored into the key as a sorted set, in a way that makes it possible to query the items with the GEOSEARCH command.  

#### GEOADD options

- **XX**: Only update elements that already exist. Never add elements.
- **NX**: Don't update already existing elements. Always add new elements.
- **CH**: Modify the return value from the number of new elements added, to the total number of elements changed (`CH` is an abbreviation of changed). Changed elements are new elements added and elements already existing for which the coordinates was updated. So elements specified in the command line having the same score as they had in the past are not counted. Note: normally, the return value of `GEOADD` only counts the number of new elements added.  

#### Return

Integer reply, specifically:
- When used without optional arguments, the number of elements added to the sorted set (excluding score updates).
- If the `CH` option is specified, the number of elements that were changed (added or updated).

#### Examples:  

```shell
127.0.0.1:6379> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
```

### GEOHASH

#### Syntax

> GEOHASH key member [member ...]

Return valid `Geohash` strings representing the position of one or more elements in a sorted set value representing a geospatial index (where elements were added using `GEOADD`).  

#### Return

Array reply, specifically:

The command returns an array where each element is the `Geohash` corresponding to each member name passed as argument to the command.

#### Examples:

```shell
127.0.0.1:6379> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
127.0.0.1:6379> GEOHASH Sicily Palermo Catania
1) "sqc8b49rny0"
2) "sqdtr74hyu0"
```

### GEOPOS

#### Syntax  

> GEOPOS key member [member ...]

Return the positions (longitude,latitude) of all the specified members of the geospatial index represented by the sorted set at key.

Given a sorted set representing a geospatial index, populated using the `GEOADD` command, it is often useful to obtain back the coordinates of specified members. When the geospatial index is populated via `GEOADD` the coordinates are converted into a 52 bit geohash, so the coordinates returned may not be exactly the ones used in order to add the elements, but small errors may be introduced.

The command can accept a variable number of arguments so it always returns an array of positions even when a single element is specified.  

#### Return  

Array reply, specifically:

The command returns an array where each element is a two elements array representing longitude and latitude (x,y) of each member name passed as argument to the command.

Non existing elements are reported as NULL elements of the array.

#### Examples

```shell
127.0.0.1:6379> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
127.0.0.1:6379> GEOPOS Sicily Palermo Catania NonExisting
1) 1) "13.36138933897018433"
   2) "38.11555639549629859"
2) 1) "15.08726745843887329"
   2) "37.50266842333162032"
3) (nil)
```

### GEODIST

Calculate the distance between two points

> GEODIST Key 

### GEORADIUS

### GEORADIUS_RO

### GEORADIUSBYMEMBER

### GEORADIUSBYMEMBER_RO

### GEOSEARCH

### GEOSEARCHSTORE

## References

[Redis Commands](https://redis.io/commands)