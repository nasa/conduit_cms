# conduit_cms

Source code will be checked in soon...
If you would like to contribute, please fork this repository and issue a pull request. 

# README

Database seeds for development are in db/seeds/development.rb.  Test pages for the following rake tasks(db:reset, edtopnav:ingest) 
will create pages in "TEST" project.

Run `rake db:reset` to load the test data.

Run `rake edtopnav:ingest` to create and load pages and redirects required for conduit web Top Navigation.  
This is a required step after every database reset.  

Run `rake edtopnav:delete` to delete pages and redirects required for conduit web Top Navigation.

Page and redirect information required for ED top navigation is configured in "spec/fixtures/ed_top_navigation.json".  
Any changes to the json file should be preceded by  running `rake edtopnav:delete` 
After updating the json file run  `rake edtopnav:ingest`


By default, this will make the local workstation user an admin in the app; if
your local username does not match your AUID, you can override this by
setting the AUID_USER environment variable. For example:

    AUID_USER=my_auid rake db:reset

You could also add `export AUID_USER=foo` to your .bash_profile to make this
change permanently.

Conduit requires PostgreSQL 9.2 or above (and will soon require 9.3 or above).

##Testing Launchpad Authentication in Dev Environment

To test authentication with Launchpad, run off any of these instead of the usual (localhost:3000):
	https://conduit.dev.earthdata.nasa.gov
	https://conduit.sit.earthdata.nasa.gov
	https://conduit.uat.earthdata.nasa.gov
	https://conduit.earthdata.nasa.gov

Add this line your /etc/hosts file depending on the environment you're currently testing.

```
127.0.0.1	conduit.dev.earthdata.nasa.gov
```

You'll also need to alter your nginx.conf file. Add

```
	ssl_certificate /usr/local/etc/nginx/cert.crt;
    ssl_certificate_key /usr/local/etc/nginx/cert.key;

    server {
        listen 443 ssl;

        server_name conduit.dev.earthdata.nasa.gov; # Change this line for a different environment

        ssl on;
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout 5m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;
        location / {
            root html;
            index index.html index.htm;

            proxy_pass http://conduit.dev.earthdata.nasa.gov:3000; # Change this line for a different environment

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
```
To start the server run "rails s -b 0.0.0.0" then go to https://conduit.dev.earthdata.nasa.gov

If you change the environment you're testing you'll have to update the following values in the application.yml file.
	SAML_SP_ISSUER_BASE
	SAML_SP_ISSUER
	SAML_SP_ACS_URL


For details about custom fields in pages that may need processing before rendering
see [API Documentation](docs/api.md)


