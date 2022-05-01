---
# An instance of the Contact widget.
widget: contact

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 130

title: Contact
subtitle:

content:
  # Automatically link email and phone or display as text?
  autolink: true

  # Email form provider
  form:
    provider: netlify
    formspree:
      id:
    netlify:
      # Enable CAPTCHA challenge to reduce spam?
      captcha: false

  # Contact details (edit or remove options as required)
  email: peng.sus@outlook.com
  phone: 888 888 88 88
  address:
    street: 集美大道1799号
    city: 厦门市
    region: 福建
    postcode: '361021'
    country: 中国
    country_code: CN
  coordinates:
    latitude: '37.4275'
    longitude: '-122.1697'
  directions: 中国科学院城市环境研究所主楼
  office_hours:
    - '周一 10:00 to 13:00'
    - '周三 09:00 to 10:00'
  appointment_url: 'https://calendly.com'


design:
  columns: '2'
---
