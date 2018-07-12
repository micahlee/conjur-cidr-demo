# Conjur Network Authentication Restrictions with CIDR

This demo showcases the capability in Conjur to restrict authentication
for a user or host to a particular network. This mitigates the risk of
credentials being used outside of their intended context or origin.

This feature is particularly relevant for host authentication, where the host
network is usually well-known, and an authentication request outside of the
host network would likely be undesired. For ease of demonstration, this example
shows the restriction with User logins.

## Running the Demo

```sh-session
$ bin/0_init
Fetching latest container images...
Pulling database ... done
Pulling conjur   ... done
Pulling admin    ... done
Pulling bad      ... done
Pulling good     ... done
```

The Conjur data key now resides in the file `./data_key`.


```sh-session
$ bin/1_start
Starting Conjur service...
20180711_cdir_demo_database_1 is up-to-date
Recreating 20180711_cdir_demo_conjur_1 ... done
Recreating 20180711_cdir_demo_bad_1    ... done
Recreating 20180711_cdir_demo_admin_1  ... done
Recreating 20180711_cdir_demo_good_1   ... done
Creating demo account "cdir-demo"...
Installing network tools in client containers...
debconf: delaying package configuration, since apt-utils is not installed
debconf: delaying package configuration, since apt-utils is not installed
```

The `admin` user info now resides in the file `./admin_info`

```sh-session
$ bin/2_load-policy
Wrote configuration to /root/.conjurrc
Please enter admin's password (it will not be echoed):
Logged in
```

The login info for the user, `alice`, is now in the file `./alice_info`

```sh-session
$ bin/3_login good
Wrote configuration to /root/.conjurrc
Please enter alice's password (it will not be echoed):
Logged in

Network configuration:
---------------------------------------------------
          inet addr:172.22.200.2  Bcast:172.22.255.255  Mask:255.255.0.0

Conjur response:
---------------------------------------------------
{
  "created_at": "2018-07-12T14:36:36.238+00:00",
  "id": "demo:variable:password",
  "owner": "demo:user:admin",
  "policy": "demo:policy:root",
  "permissions": [
    {
      "privilege": "read",
      "role": "demo:user:alice",
      "policy": "demo:policy:root"
    }
  ],
  "annotations": [

  ],
  "secrets": [

  ]
}
```

```sh-session
$ bin/3_login bad
Wrote configuration to /root/.conjurrc
Please enter alice's password (it will not be echoed):
Logged in

Network configuration:
---------------------------------------------------
          inet addr:172.22.100.1  Bcast:172.22.255.255  Mask:255.255.0.0

Conjur response:
---------------------------------------------------
Unable to authenticate with Conjur. Please check your credentials.
```
