# Overview


# Configuration variables

* Create the App Engine configuration file
    ```
    cp app.yaml.sample app.yaml
    ```
* Edit the `app.yaml` file with your favorite editor and set the following environement variables appropriately:

```
  SMTP_SERVER_ADDR: "smtp.mailgun.org:587"
  SMTP_VERITY_CERT: true
  SMTP_CLIENT_USERNAME: "postmaster@example.com"
  SMTP_CLIENT_PASSWORD: "postmasterSecretPassWord"
  CONTACT_REPLY_EMAIL: "noreply@example.com"
  CONTACT_REPLY_CC_EMAIL: "contact@example.com"
  DEMO_URL: "https://demo.example.com/"
  ALLOWED_ORIGIN_DOMAIN: "example.com"
```


## Required HTTP Headers

* `Origin`
* `Referer`

## SMTP
https://cloud.google.com/compute/docs/tutorials/sending-mail/using-mailgun?hl=fr

## Test
```
curl -H'Origin: http://realopinsight.com'  \
    -H'Referer: realopinsight.com' \
    -H'Content-Type: application/x-www-form-urlencoded' \
    -d 'target=contact' \
    -XPOST https://hugo-mx-gateway.ew.r.appspot.com/sendmail 
```
# Build
```sh
make build
```

# Hugo Contact Form
See `./model/hugo-contact-form.html`.


```
<div id="reply-message"></div>
<div>
   <fieldset>
      <legend>Please fill in the form to submit your request</legend>
      <form action="https://contact-request-endpoint/" method="post">
         <div class="form-item">
            <label for="name">Name</label>
            <input type="text" name="name" id="name"  placeholder="Mr. Smith" />
         </div>
         <div class="form-item">
            <label for="email">Email <span class="req"></span></label>
            <input type="text" name="email" id="email"  class="required email" placeholder="smith@company.com" />
         </div>
         <div class="form-item">
            <label for="organization">Organization</label>
            <input type="text" name="organization" id="organization"  placeholder="Company, Inc." />
         </div>
         {{ if in .Params.tags "contact" }}  
         <div class="form-item">
            <label for="subject">Subject</label>
            <input type="text" name="subject" id="subject"  value="" placeholder="Need help or expertise?" />
            <input type="hidden" name="target" id="target"  value="contact" />
         </div>
         <div class="form-item">
            <label for="message">Message</label>
            <textarea  rows="6" name="message" id="message"  placeholder="Please add details concerning your request."></textarea>
         </div>
         {{ else }}    
         <div class="form-item">
            <input type="hidden" name="subject" id="subject"  value="Your Access to Product Demo!" />
            <input type="hidden" name="target" id="target"  value="demo" />
         </div>
         {{ end }}
         <input  class="button"  type="submit" value="Submit">
      </form>
   </fieldset>
</div>
```