# ukagaka uses the metainfo standard  

## What is metainfo?  

metainfo is an extensible standard for describing ukagaka, which contains the basic information needed for web applications  

## Why use metainfo?  

Take GCS as an example
Consider the information architecture required by the current version of GCS. When you need to customise the preview image of your ghost in CGS, you need to store one or two apng files in `shell/matser` that the user won't even use  
This not only disrupts the project layout, but also allows files with different uses to be mixed together, which is not conducive to project maintenance  
Consider the redirection provided by the current version of GCS, which only redirects to the author's home page and lacks customisation and customisation.  
metainfo was created to solve these problems  

## Structural criteria for metainfo  

### Storage location  

We recommend storing the metainfo folder in any cloud storage directory that is not in the local ghost folder to keep it up to date and does not take up local storage space for the user  
If your ghost has a separate project on github, we recommend storing the metainfo folder in the root of the default branch of the project  
For example `https://github.com/Taromati2/Taromati2`, as a standard, the metainfo folder should be stored in `https://github.com/Taromati2/Taromati2/tree/master/.ukagaka` (where `master` is the default branch)  

### Related tags  

If your ghost has a separate project on github and a metainfo folder, we recommend tagging the project with `ukagaka-ghost` so that some web applications can recognise your ghost  

### File structure  

```filetree
/.ukagaka
    ├── descript.txt
    ├── /preview
    │   ├── sakura.png
    │   └── kero.png
    ├── icon.png
    ├── /links
    └── /infos
```

or

```filetree
/.ukagaka
    └── jump_to.txt
```

#### Introduction to the files  

- `descript.txt`: ghost's description file, used to provide basic information in the web application  
- `preview`: ghost's preview folder, used to provide preview images in the web application (not mandatory)  
- `icon.png`: ghost's icon file, used to provide icons in the web application (not mandatory)  
- `links`: ghost's links folder, used to provide expandable and relevant link information in the web application (not mandatory)
- `infos`: ghost's information folder, used to provide expandable additional information in the web application (not mandatory)
- `jump_to.txt`: meta information folder jump file for redirecting to a new meta information folder (only used if the current folder is deprecated)  

#### files content description

##### `preview`

You can store any number of png files in the `preview` folder, but we recommend that you store at least two png files, named `sakura.png` and `kero.png` respectively  
If you want to provide dynamic previews, you can store them in the `preview` folder as `sakura.apng` and `kero.apng` respectively.  
If your ghost has more than one kero, you can add additional names like this: `kero1.png`, `kero2.png`, `kero3.png`, etc.  

For web application developers, you need to note that the entire `preview` folder is optional and any content within it is also optional  

##### `descript.txt`

The contents of the `descript.txt` file are mandatory, it should be `UTF8` encoded and follow this format.  

```txt
//meta info
type,ghost
name,Taromati2
uuid,R5dVNluBvKjtQqjP0dAuoA==
sakura.name,橘花
kero.name,斗和
has_terms,1
icon,https://github.com/Taromati2/other/raw/master/discord_app/kikka_eye_open.png
homeurl,https://raw.githubusercontent.com/Taromati2/stable-mirror/master/Taromati2/
craftman,8 and other contributors
craftmanurl,https://github.com/Taromati2/Taromati2/blob/master/docs/CONTRIBUTORS.md
languages,Simplified Chinese	//,Japanese,Engilsh may be available in future
```

Any `//` that does not start with `://` up to the end of the line will be treated as a comment and should not be parsed  
`//meta info` as a start is required  
The following options are required.  
`type`: type (for later standard expansion), must be `ghost`  
`name`: name  
`craftman`: producer  
`craftmanurl`: link to the producer  
`uuid`: unique identifier, see [UUID standard](#uuid) for details  
`languages`: the natural languages supported by ghost, separated by `,` if there are more than one  
When `type` is `ghost`, the following options are required.  
`sakura.name`: the name of the enabler `\0`  
`kero.name` is optional (not required), considering that ghost does not necessarily have a servant.  
`kero.name`: the name of the maid `\1` (if there are multiple maids, they can be distinguished by `kero1.name`, `kero2.name`, etc.)  

Not required.
`has_terms`: whether or not there are terms (0 for no, 1 for yes, default 0 if not written)  
`icon`: icon link (unwritten defaults to `icon.png` in the metainfo folder (if the file exists))  
`uuid_base`: uuid base for preventing uuid collisions (see [UUID standard](#uuid) for details)  

##### `links`

In the `links` folder you can store as many txt files as you like to hold the relevant link information, the files specified (but not required) in the standard are

- `mirror_repo.txt`: if the contents of your ghost files are not updated in this repository, you can write a link to your mirror repository in this file  
  The link should be a github repository link  
- `nar_release_repo.txt`: if your ghost's nar files are not released in this repository, you can write a link to your nar release repository in this file  
  The link should be a github repository link

The rest of the files are left to third parties for standard extensions, and the possibility of adding new standard-specified files can be ruled out later.  
Recommended third-party extensions that are not generic should include the name of the web application or an abbreviation in the file name, e.g. `GCS_link.txt`

##### `infos`

In the `infos` folder you can store as many txt files as you need to store information about the web application  

## Directing to the metainfo folder via ghost  

Ghost developers are encouraged to write a `metainfo` field in `ghost/descript.txt` followed by the url to the metainfo folder so that specific applications can be directed to the metainfo folder via ghost
You can also write the `githubrepo` field in `ghost/descript.txt` followed by the url of the github project, both of which have the same effect and can co-exist  
Example.

```txt
//...
mousecursor.grip,./img/curs/hand_grip.cur
//
metainfo,https://raw.githubusercontent.com/Taromati2/Taromati2/master/.ukagaka/
githubrepo,https://github.com/Taromati2/Taromati2
```

A ghost without metainfo can append metainfo and uuid in this way  

## uuid

Considering that different versions of the same ghost or different branches of the same ghost may exist, uuid is necessary  
The web application can use uuid to distinguish between virtually different ghosts with the same name  

### uuid generation  

If you don't care about uuid principle, you can use [uuid-genner](https://github.com/ukatech/metainfo-standard/tree/master/uuid-genner) to generate uuid without any code.  
To generate uuid, you need to get the url of the current ghost's metainfo folder (encoded with `UTF8` and no newlines at the end), then encrypt it with md5 and encode the binary result with base64  
For example, the uuid generated by `https://raw.githubusercontent.com/Taromati2/Taromati2/master/.ukagaka/` is `R5dVNluBvKjtQqjP0dAuoA==`  
If there is no metainfo folder in ghost, the uuid will be base64 encoded with the binary result `0`.  
If there is `uuid_base` field in metainfo, add it directly in front of the uuid obtained in the previous step  
The `uuid_base` field causes the uuid to be indefinite, but allows the application to reject uuids that are longer than 64 bits  

### uuid checksum  

The web application should validate the uuid of a new ghost as it is captured or received to check the legitimacy of the information to avoid any possibility of confusing different ghosts  
The uuid check should access the metainfo folder and check if there is a `jump_to.txt` file, if so the application needs to update the url of the metainfo folder for this ghost and its uuid  
Given that the uuid may change due to changes in the address of the metainfo folder, the same ghost may have multiple historical uuids.  
Application to application specific ghost related requests and logic builds should not use uuid to distinguish between ghosts, but should include the url of the metainfo folder so that the uuid checksum can be based on the latest information  
Applications are allowed to cache past uuids of ghosts to enable fast ghost matching, but should update the cache if the match fails  
If the checksum fails, the application should reject the registration associated with this ghost  

**Important:** A ghost without uuid should not be confused with a ghost of the same name with uuid  
To add uuid to an existing ghost, please refer to [redirect to metainfo folder via ghost](#Directing-to-the-metainfo-folder-via-ghost)  

### Disadvantages

uuid can only protect existing ghosts with uuid from being confused or toppled, but cannot protect ghosts without uuid in history from being toppled  
