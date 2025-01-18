Credentials are saved in the file "Credentials.csv" in the Registry's home folder.

#### Credentials for your own Tatin Server

If you run your own Tatin Server, we suggest that you create a UUID and use that as an API key. 

For an API key to be accepted by a Tatin Server, it must be added to a file `Credentials.txt` in the Registry's root directory. 

Make sure that you specify it as either

```
<group-name>,<api-key>
```

or

```
*,<api-key>
```

If the server finds such a file it will perform the following actions:

* Take the data and convert it to a different format
* Delete rows from `Credential.csv` that share a group name with `Credentials.txt`
* Create a Salt for every API key in `Credentials.txt`
* Convert every API-key and its Salt into a hash and add them together with the according group name to `Credentials.csv`
* Delete the file `Credentials.txt`

The format of the file `Credentials.csv` is:

```
<group-name>,<api-key-hash>,<salt>
*,<api-key-hash>,<salt>
*
```

* In the first case, anybody who provides the API key the hash was produced from can publish packages for that group.
* In the second case, the password the hash was created from is a kind of master password: it allows the creation of packages with _any_ group name, except any group that was already handled by then of course.
* The third case means that no API key is required for any (remaining) group(s).

The different scenarios can be mixed:

```
group1,<hash1,<salt1>
group2,<hash2,<salt2>
*,{hash3>,<salt3>
```

This means that one can only publish packages with the group name...

* `<group1>` by using the API key "hash1" was generated from...
* `<group2>` by using the API key "hash2" was generated from...

and anything else by using the API key "hash3" was created from.

Note that `*` or`*,` or `*=` all mean that no API key is required. On its own, it's the same as having no credentials file, but it can be useful together with other group names:

```
<group1>,<hash1>
<group2>,<hash2>
*
```

This is interpreted as "require API keys for the groups `<group1>` and `<group2>` but allow anything else without an API key".

Finally, you can allow anybody to publish packages under a particular group name without providing an API key:

```
<group1>,<hash1>,<email-address>
<group2>,
*,<hash3>
```

This means:

* In order to publish anything to the group "group1" you must provide the API key "hash1" was generated from

* You may publish packages to the group "group2" without an API key 

* For any group name but `<group1>` and `<group2>` you must specify the API key "hash3" was generated from


##### Editing the file "Credentials.csv"

There is one reason why you might need to change the file `Credentials.csv`: when a group name must be deleted for some reason.

If a new group needs to be added, or a new API key needs to be assigned to an existing group, you must create a file `Credentials.txt`, see above.

##### Comments

The files `Credentials.txt` as well as `Credentials.csv` both allow comment lines: any line that has a `;` as the very first character is regarded a comment.
