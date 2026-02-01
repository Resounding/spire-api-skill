# Email API

Base Path: `/api/v2/companies/{company-name}`

---

## Email Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email` | Get list of emails |

---

## Email Batches

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email-batches` | Get list of email batches |
| POST | `/email-batches` | Create an email batch |
| GET | `/email-batches/{id}` | Get email batch by ID |
| PUT | `/email-batches/{id}` | Update email batch by ID |
| DELETE | `/email-batches/{id}` | Delete email batch by ID |
| POST | `/email-batches/{id}/send` | Send email batch |

---

## Email Batch Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email-batches/{id}/contacts` | Get list of email batch contacts |

---

## Email Batch Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email-batches/{id}/messages` | Get list of email batch messages |

---

## Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email-messages` | Get list of email messages |
| POST | `/email-messages` | Create an email message |
| GET | `/email-messages/{id}` | Get email message by ID |
| PUT | `/email-messages/{id}` | Update email message by ID |
| DELETE | `/email-messages/{id}` | Delete email message by ID |
| POST | `/email-messages/{id}/send` | Send email message |

---

## Email Message Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email-messages/{id}/contacts` | Get list of email message contacts |

---

## Email Message Attachments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email-messages/{id}/attachments` | Get list of email message attachments |
| POST | `/email-messages/{id}/attachments` | Create email message attachment |
| GET | `/email-messages/{message-id}/attachments/{attachment-id}` | Get attachment by ID |
| PUT | `/email-messages/{message-id}/attachments/{attachment-id}` | Update attachment by ID |
| DELETE | `/email-messages/{message-id}/attachments/{attachment-id}` | Delete attachment by ID |
| GET | `/email-messages/{message-id}/attachments/{attachment-id}/data` | Get attachment data |

---

## Email Templates

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/email-templates` | Get list of email templates |
| POST | `/email-templates` | Create an email template |
| GET | `/email-templates/{id}` | Get email template by ID |
| PUT | `/email-templates/{id}` | Update email template by ID |
| DELETE | `/email-templates/{id}` | Delete email template by ID |

---

## Example Queries

```bash
# Get all email messages
GET /email-messages

# Create an email message
POST /email-messages
Content-Type: application/json
{
  "to": "customer@example.com",
  "subject": "Order Confirmation",
  "body": "Thank you for your order...",
  ...
}

# Send an email message
POST /email-messages/12345/send

# Get email templates
GET /email-templates

# Create an email batch
POST /email-batches
Content-Type: application/json
{
  "template": {"id": 1},
  ...
}

# Send email batch
POST /email-batches/12345/send
```
