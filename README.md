# Usabi chef cookbooks

Basic cookbooks for our servers/workstations.

## Usage

We use knife-solo for simplicity (we don't need whole chef infrastructure).

Cookbooks are installed with librarian-chef (like bundler for ruby. Installs cookbooks from Cheffile).

#### Install librarian-chef

```
gem install -V librarian-chef
```

#### Download cookbooks

```
librarian-chef install
```

#### Bootstrap new node

First add node definition.

e.g. If we need a new tmate server (https://tmate.io/)

Create new nodes/server_name.json file (choose suitable name):

```
{
  "run_list": [
    "recipe[tmate-slave]"
  ],
  "automatic": {
    "ipaddress": "192.168.1.2"
  },
  "normal": {
    "port": "9922",
    "host": "128.199.60.151"
  }
}
```

* run_list: Declares recipes to use in server
* ip_address: Ip that chef will use to reach to node (ssh)
* normal (optional): Attributes for the recipes

Now we can prepare node
```
knife solo prepare user@server_name
```

And upload all the cookbooks (executes `librarian-chef` install too, updating cookbooks)
```
knife solo cook user@server_name
```

If all goes well the server is configured and ready to use

## Other considerations

#### Add new cookbooks

We can add new cookbooks to the repo with librarian-chef

Simply add new cookbook to Cheffile and run `librarian-chef install` again
