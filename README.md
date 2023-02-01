# Utilizing Google Maps APIs to Make Online Purchases Delightful

[Application Image Will Go Here]

## Overview

To ensure that a customer's experience in an ecommerce app is good, a few things have been factored in:

- A vendor's shop location is saved in the database during registration. It is advisable that vendors register from their shop locations

- This data will be used by the customer to search for vendors near him. Vendors within a customer's vicinity will be found in the search query.

- Ideally, a map with vendor markers will be displayed to help the customer visualize the nearest vendors.

## Google Maps Geolocation API

[Google Maps Geolocation API](https://developers.google.com/maps/documentation/geolocation/overview) can be used to return a location and accuracy radius that a mobile client can detect where communication is done over HTTPS using POST. Both request and response are formatted as JSON, and the content type of both is `application/json`.

> **Geolocate** means identify the geographical location of (a person or device) by means of digital information processed via the internet.

```python
import requests


@app.route('/register-vendor')
def register_vendor():
    # Send a POST geolocation request
    url = "https://www.googleapis.com/geolocation/v1/geolocate?key=" + app.config['GOOGLE_MAPS_API_KEY']
    response = requests.post(url)
    data = response.json()
    lat = data['location']['lat']
    lng = data['location']['lng']

    # Update database
    form = VendorRegistrationForm()
    if form.validate_on_submit():
        vendor = Vendor(latitude=lat, longitude=lng)
        db.session.add(vendor)
        db.session.commit()
        return redirect(url_for('register_vendor'))
```