# CRM API

Base Path: `/api/v2/companies/{company-name}`

---

## CRM Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/crm` | Get CRM root |

---

## CRM Note Types

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/crm/note-types` | Get list of note types |

---

## CRM Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/crm/notes` | Get list of notes |
| POST | `/crm/notes` | Create a note |
| GET | `/crm/notes/{id}` | Get note by ID |
| PUT | `/crm/notes/{id}` | Update note by ID |
| DELETE | `/crm/notes/{id}` | Delete note by ID |

---

## CRM Note Attachments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/crm/notes/{id}/attachments` | Get list of note attachments |
| POST | `/crm/notes/{id}/attachments` | Create a note attachment |
| GET | `/crm/notes/{note-id}/attachments/{attachment-id}` | Get note attachment by ID |
| PUT | `/crm/notes/{note-id}/attachments/{attachment-id}` | Update note attachment by ID |
| DELETE | `/crm/notes/{note-id}/attachments/{attachment-id}` | Delete note attachment by ID |
| GET | `/crm/notes/{note-id}/attachments/{attachment-id}/data` | Get attachment file data |

---

## Example Queries

```bash
# Get CRM root
GET /crm

# Get all notes
GET /crm/notes

# Create a note
POST /crm/notes
Content-Type: application/json
{
  "noteType": {"id": 1},
  "subject": "Follow up call",
  "body": "Discussed pricing options...",
  ...
}

# Get note types
GET /crm/note-types

# Get note attachments
GET /crm/notes/12345/attachments

# Upload attachment
POST /crm/notes/12345/attachments
Content-Type: multipart/form-data
...

# Download attachment data
GET /crm/notes/12345/attachments/67890/data
```
