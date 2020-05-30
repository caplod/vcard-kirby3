# VCard Plugin for Kirby 3
Easily generate VCard for your contact profiles

![](https://avatars3.githubusercontent.com/u/28547180?s=50&v=4)

## Requirements
- Kirby V3
- PHP 7.1+
- ```get_headers()``` configurated on the server by ```allow_url_fopen=1```

## How to Install
This is a Wrapper for the [jeroendesloovere/vcard](https://github.com/jeroendesloovere/vcard) library, you can install the latest version with Composer:
```
composer require isaactopo/vcard-kirby3
```

## Usage
After installing the VCard Plugin you need to create a controller for your page, for example a profile page: ```site/controllers/profile.php``` : With this controller you can pass a variable to your controller like: yourdomain.com/profile/vcard:download it could be ```download```or whatever you like if the param ```vcard``` is present
```php
<?php

return function ($page, $pages, $site, $kirby) {
    if ($vcard = param('vcard')) {

        // Get the VCard Instance
        $vcard = $site->vcard();

        // Define Variables
        $additional = '';
        $prefix = '';
        $suffix = '';
        $lastname = '';

        // Get Your data from your Fields
        $firstname = $page->firstName();

        // add personal data
        $vcard->addName($lastname, $firstname, $additional, $prefix, $suffix);

        // add work data
        $vcard->addCompany('DaCompany');
        $vcard->addJobtitle('Web Developer');
        $vcard->addRole('Code reviewer');
        $vcard->addEmail('info@topo.bz');
        $vcard->addPhoneNumber(666666666, 'PREF;WORK');
        $vcard->addPhoneNumber(93888666222, 'WORK');
        $vcard->addAddress(null, null, 'street', 'worktown', null, 'workpostcode', 'Belgium');
        $vcard->addLabel('street, worktown, workpostcode Belgium');
        $vcard->addURL('http://topo.bz');

        // Add a photo
        if($img = $page->profilePicture()->toFile()){
            $img  = $img->crop(400, 400, 85);
            $vcard->addPhoto($img->url());
        }

        return $vcard->download();
    }
}
```

For Debugging you can echo the VCard as a String
```php
// return vcard as a string
echo $vcard->getOutput();
```

You can save the VCard on Dick too:
```php
// save vcard on disk
$vcard->setSavePath('/path/to/directory');
$vcard->save();
```

## License
This plugin is free but [Kirby](https://getkirby.com) needs a license

---
### Credits
- [Isaac Bordons](https://github.com/isaactopo)
- [Jeroen Desloovere](https://github.com/jeroendesloovere)