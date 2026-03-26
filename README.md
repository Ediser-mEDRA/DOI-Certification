# DOI Certification
The Certification web form is part of the mEDRA interface used to define and manage metadata associated with a DOI, particularly for specific content types not created by an AI and their structured description.

Table of contents
=================
  * [Table of contents](#table-of-contents)
  * [Certification service overview](#service-overview)
    * [Features of the Interface](#main-features-of-the-interface)
  * [Message Information](#message-information)
  * [DOI Data](#doi-data)
    - [DOI](#doi)
    - [URL](#url)
  * [Certification Information](#certification-information)
    * [Certification Data](#certification-data)
      - [Certification Type](#certification-type)
      - [Certification Name](#certification-name)
    * [Subject Data](#subject-data)
      - [Subject Type](#subject-type)
      - [Subject of the Certification](#subject-of-the-certification)
      - [Subject ID Type](#subject-id-type)
      - [Subject ID Value](#subject-id-value)
      - [Subject ID Value Format Patterns](#subject-id-value-format-patterns)
    - [Status & Dates](#status--dates)
    - [Certification Status](#certification-status)
    - [Start Date](#start-date)
    - [End Date](#end-date)
  - [Issuer & Release](#issuer--release)
    - [Assertion Issuer](#assertion-issuer)
    - [Assertion Release Date](#assertion-release-date)
  - [Scheme & Description](#scheme--description)
    - [Certification Scheme Name](#certification-scheme-name)
    - [Additional Description](#additional-description)
  * [Confirm Data and/or Validation](#confirm-data-andor-validation)
    

## Service overview
The service provides a multi‑step, wizard‑based web interface supplied by the mEDRA (Multilingual European DOI Registration Agency) application. This interface enables the registration of DOI (Digital Object Identifier) metadata used to certify that a publication, publisher, or digital resource is human‑created. 

Within the mEDRA system's existing workflow, an additional link is integrated to allow authorised users to initiate the mEDRA certification process directly from the web forms. The visibility and accessibility of this link are controlled through a dedicated permission (grant) managed in IANUS. Only users explicitly assigned this permission will be able to access and invoke the certification feature; for all other users, the link will remain hidden.

  ### Main Features of the Interface
  * Step-by-step wizard with progress indicator;
  * Real-time validation with error messages at each tab;
  * Required field enforcement marked with red asterisk (*);
  * Real-time of all validations with error messages on the Confirm Data screen;

## Message Information
In the initial step of the wizard, the system gathers the registrant's identity details and sender information. This data is required to associate the DOI registration request with the submitting organization and to ensure that status updates and result notifications are routed to the appropriate recipients.

  ### `From Company` 
  The full legal name of the organization submitting the DOI registration.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="text">` |
  | **Required** | ✅ Yes |
  | **Validation type** | Non-empty string |
  | **Notes** | Changing this field auto-copies the value to `DOI Registrant` if `DOI   Registrant` is currently empty |

  ### `From Email`
  The email address of the registrant. This address will receive the final registration   result notification.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="email">` |
  | **Required** | ✅ Yes |
  | **Validation type** | Email — must match `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` |
  | **Notes** | Results/confirmation e-mail are sent to this address |

  ### `DOI Registrant`
  The name of the entity that owns or manages the DOI namespace being registered.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="text">` |
  | **Required** | ✅ Yes |
  | **Validation type** | Non-empty string |
  | **Notes** | Auto-populated from `fromCompany` on change if blank |

## DOI Data
This step collects the essential Digital Object Identifier (DOI) metadata. Users must provide both the DOI string and the target URL to which the DOI resolves. An inline reference link to the official DOI creation guidelines is provided to ensure correct formatting and compliance with DOI syntax rules.

  ### `DOI`
  The unique identifier string for the resource. Must conform to the mEDRA DOI naming conventions.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="text">` |
  | **Required** | ✅ Yes |
  | **Validation type** | Pattern match |
  | **Pattern** | `/^10\.[0-9]{4,9}\/[^\s&<>'"]{1,200}$/i` |
  | **Pattern description** | Must start with `10.` followed by 4–9 digits, a `/`, and 1–200 characters (no whitespace, `&`, `<`, `>`, `'`, `"`) |
  | **Readonly condition** | Field is `readonly` when updating an existing DOI) |

  ### `URL`
  The resolution URL, the web address the DOI will redirect to when resolved via https://doi.org.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="url">` *(semantically should be `url`)* |
  | **Max length** | 300 characters |
  | **Required** | ✅ Yes |
  | **Validation type** | URL |

## Certification Information
In this step, the most complex stage of the wizard, the system collects structured metadata describing the certification or assertion associated with the DOI metadata. The interface presents multiple required and optional metadata fields.

  ### Certification Data

  #### `Certification Type`
  A dropdown to specify the category of certification (e.g., AI, Human created).
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<select>` |
  | **Required** | ✅ Yes |
  | **Notes** | The `value` attribute becomes the `option` attribute in the XML; the visible label becomes the element text content |

  #### `Certification Name`
  A second dropdown to select the specific certification name (standard).
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<select>` |
  | **Required** | ✅ Yes |

  ### Subject Data

  #### `Subject Type`

  | Attribute | Value |
  |---|---|
  | **HTML type** | `<select>` |
  | **Required** | ⚠️ Conditionally — required when `Subject of the Certification` is filled |
  | **Options** | `creation`, `event`, `organization`, `people`, `place` (hardcoded sequence in XSLT list) |
  | **Cross-field rule** | If `Subject of the Certification` has a value AND `Subject Type` is empty, then validation error is thrown |

  #### `Subject of the Certification`
  Subject of the Certification: Identifies the entity being certified, typically a publisher name
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="text">` |
  | **Required** | ✅ Yes |
  | **Validation type** | Non-empty string |
  | **Notes** | Represents the certified entity name (publisher, organisation, person, etc.) |
  
  #### `Subject ID Type`
  Subject ID Type: providing a unique identifier for the subject (e.g., ISNI, ORCID).
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<select>` |
  | **Required** | ⚠️ Conditionally — required when `Subject ID Value` has a value |
  | **Options** | `ISNI`, `ISBN`, `ISSN`, `ORCID`, `ROR`, `WIKI`, `DOI`, `VAT`, `LEI` |
  | **Side effect on change** | Resets `subject ID Value` field, updates placeholder, shows/hides URL prefix for ORCID and ROR |
  | **Cross-field rule** | If `Subject ID Value` is filled AND `Subject ID Type` is empty, then a validation error is thrown on `Subject ID Type` |
  

  #### `Subject ID Value`

  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="text">` |
  | **Required** | ⚠️ Conditionally, required when `Subject ID Type` is selected |
  | **Validation type** | Format pattern — depends on `Subject ID Type` (see table below) |
  | **Cross-field rule** | If `Subject ID Type` is selected AND `Subject ID Value` is empty, then a validation error is thrown on `Subject ID` |

  #### Subject ID Value Format Patterns

  | Type | Pattern | Hint shown to user |
  |---|---|---|
  | `DOI` | `/^10\.[0-9]{4,9}\/[^\s&<>'"]{1,200}$/i` | `Es: 10.1234/example-doi` |
  | `ISBN` | `/^(?:97[89]-?)?(?:\d-?){9}[\dX]$/i` | `Es: 978-3-16-148410-0 oppure 0-306-40615-2` |
  | `ISNI` | `/^\d{4}[ -]?\d{4}[ -]?\d{4}[ -]?\d{3}[\dX]$/i` | `Es: 0000 0001 2345 6789` |
  | `ISSN` | `/^\d{4}-\d{3}[\dX]$/i` | `Es: 1234-567X` |
  | `ORCID` | `/^(?:https?:\/\/orcid\.org\/)?\d{4}-\d{4}-\d{4}-\d{3}[\dX]$/i` | `Es: 0000-0002-1825-0097` |
  | `ROR` | `/^(?:https?:\/\/ror\.org\/)?0[a-z0-9]{6}\d{2}$/i` | `Es: https://ror.org/02mhbdp94` |
  | `WIKI` | `/^Q\d+$/i` | `Es: Q76 (solo QID Wikidata)` |
  | `LEI` | `/^[A-Z0-9]{18}\d{2}$/i` | `Es: 7H6GLXDRUGQFU57RNE97 (20 caratteri)` |
  | `VAT` | `/^[A-Z]{2}[A-Z0-9]{2,12}$/i` | `Es: IT12345678901` |
  
  Pattern validation only fires when **both** `Subject ID Type` and `Subject ID Value` have values.

  ### Status & Dates

  #### `Certification Status`

  | Attribute | Value |
  |---|---|
  | **HTML type** | `<select>` |
  | **Required** | ✅ Yes |
  | **Options** | See table below |

  | Code | Label |
  |---|---|
  | `01` | proposed |
  | `02` | verified |
  | `03` | valid |
  | `04` | refuted |
  | `05` | under review |
  | `06` | withdrawn |

  #### `Start Date`
  Validity period of the certification
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="date">` |
  | **Required** | ✅ Yes |
  | **Format** | `YYYY-MM-DD` (browser native date picker) |
  | **Cross-field rule** | Must be strictly before `Enad Date` when both are filled |

  #### `End Date`
  Validity period of the certification
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="date">` |
  | **Required** | ❌ Optional |
  | **Format** | `YYYY-MM-DD` |
  | **Cross-field rule** | Must be strictly after `Start Date` when both are filled; if `End Date ≤ Start Date`, then and error is thrown and shown under `End Date` |

  ### Issuer & Release

  #### `Assertion Issuer`
  Issuer (required): The authority or organization issuing the certification assertion
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<select>` |
  | **Required** | ✅ Yes |
  
  #### `Assertion Release Date`
  Release date (optional): The date the assertion was officially released or published.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<input type="date">` |
  | **Required** | ❌ Optional |
  | **Format** | `YYYY-MM-DD` |

  ### Scheme & Description

  #### `Certification Scheme Name`
  Scheme Name (required): A multi-select dropdown — at least one scheme must be chosen.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<select multiple>` |
  | **Widget wrapper** | Dropdown (search + list) |
  | **Required** | ✅ At least one option must be selected |
  | **Widget features** | Search filter, Select All, Clear All, item display, keyboard (Enter/Space/Escape) |

  #### `description`
  Additional Description. A free-text area for supplementary notes or remarks.
  | Attribute | Value |
  |---|---|
  | **HTML type** | `<textarea>` |
  | **Required** | ❌ Optional |

## Confirm Data and/or Validation
The final step displays a comprehensive review screen. If any required fields were omitted or completed incorrectly in earlier steps, the system renders a validation error panel that enumerates each issue and the field to which it applies. Once all validation errors have been resolved, the user can finalize the workflow by selecting the Submit button available on this screen.
Here is a walkthrough of the full success flow when the form is submitted:
### Submit button click
The user clicks the button, if no errors are found, a javascript function is called to construct the data to be sent
### XML payload construction
From all form fields across the three data tabs: sender info (Tab 1), DOI data (Tab 2), and certification details (Tab 3), the data assembles a DOIRegistrationMessage XML string
### POST request
The XML is sent via ajax as a POST to the `/sevlet/user/doidata` endpoint with `Content-Type: application/xml`
### Success response handling
If the server responds successfully, the JSON response is parsed and its `Http medra code` field is checked:
 #### if 200 
 A modal dialog is called, populating the modal body with a success message, submission details, and optionally a confirmation email address, a submission ID, and a "New assertion" shortcut button pre-filled with the DOI's metadata.
 #### Otherwise
 Inside the same former modal dialog, the modal body is populated with HTTP code, error code, and description of the error
### Post-modal actions
The user has three exit paths from the modal dialog:
 #### New assertion with [DOI] metadata
 Which jumps directly to Tab 2 (DOI Data) with the DOI field pre-focused, allowing the user to quickly register a follow-up assertion reusing the previous DOI's context. The following input are resetted: DOI, URL and Certification Scheme name
 #### New Blank Assertion 
 Reset the entire form, returns to Tab 1 (Message Information), but stays on the page, ready for a new submission.
 #### Exit 
 Resets the entire form and redirects to `/[lang]/reserved/editor.htm`

```
User clicks Submit
       │
       │
       ▼
Build Xml Payload  ← assembles DOIRegistrationMessage XML from all tabs
       │
       ▼
POST /servlet/user/doidata
Content-Type: application/xml
       │
       ├── HTTP 200 ──→ Parse JSON response
       │                      │
       │               medra code = 200 ──→ 🟢 Success modal
       │               medra code ≠ 200 ──→ 🔴 Error details inside same modal
       │
       └── HTTP error ──→ 🔴 Error modal with error message
```

## Field Quick Reference Table

| Field ID | Tab | HTML Type | Required | Validation |
|---|---|---|---|---|
| `From Company` | 1 | text | ✅ | non-empty | 
| `From Email` | 1 | email | ✅ | email regex | 
| `DOI Registrant` | 1 | text | ✅ | non-empty |
| `DOI` | 2 | text | ✅ | DOI pattern | 
| `URL` | 2 | url | ✅ | valid URL | 
| `Certification Type` | 3 | select | ✅ | non-empty | 
| `Certification Name` | 3 | select | ✅ | non-empty | 
| `Subject Type` | 3 | select | ⚠️ cond. | non-empty if Subject of the Certification filled | 
| `Subject of the Certification` | 3 | text | ✅ | non-empty | 
| `Subject ID Type` | 3 | select | ⚠️ cond. | non-empty if Subject ID Value filled | 
| `Subject ID Value` | 3 | text | ⚠️ cond. | format pattern per Subject ID Type | 
| `Certification Status` | 3 | select | ✅ | non-empty | 
| `Start Date` | 3 | date | ✅ | valid date, < endDate | 
| `End Date` | 3 | date | ❌ | > startDate if present | 
| `Assertion Issuer` | 3 | select | ✅ | non-empty | 
| `Assertion Release Date` | 3 | date | ❌ | valid date | 
| `Certification Scheme Name` | 3 | select multiple | ✅ (≥1) | at least one selected | 
| `Additional Description` | 3 | textarea | ❌ | none | 
