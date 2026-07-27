# Admin Moderation Specification

## Purpose

Allow authorised administrators to safely moderate the community, investigate
member activity, and access operational information.

## Requirements

### Requirement: Administrator-only access

The system SHALL restrict administration tools and administration APIs to
authorised administrators.

#### Scenario: Administrator opens the dashboard

- **WHEN** an authorised administrator opens the administration dashboard
- **THEN** the dashboard is displayed

#### Scenario: Regular member requests an administration API

- **WHEN** a regular member requests an administration API
- **THEN** the system denies access

### Requirement: Administration dashboard overview

The system SHALL provide authorised administrators with a dashboard that
combines member search, moderation-tool navigation, and recent community
activity.

#### Scenario: Administrator opens the dashboard with recent activity

- **WHEN** an authorised administrator opens the administration dashboard
- **THEN** the dashboard displays the ten most active messengers from the previous seven days
- **AND** the ten most recent negative thread votes
- **AND** the ten most recent experiences with a negative recommendation

#### Scenario: Administrator opens a review from the dashboard

- **WHEN** an authorised administrator selects a recent thread vote
- **THEN** the system opens the associated message-inspection view

### Requirement: Member search and role filtering

The system SHALL let authorised administrators search for members and list
members with a selected moderation role. Search queries SHALL ignore leading
and trailing whitespace, and whitespace between query words SHALL be optional
when matching stored member data. Member collections SHALL be paginated and
server-side sortable by name, username, email address, signup date, or current
IP address. The selected sort order SHALL be retained while an administrator
moves between pages.

#### Scenario: Administrator searches for a member

- **WHEN** an authorised administrator searches using a valid member query
- **THEN** the first page of matching member records is displayed

#### Scenario: Administrator searches with surrounding whitespace

- **WHEN** an authorised administrator searches using a valid member query
  with leading or trailing whitespace
- **THEN** the system searches using the trimmed query

#### Scenario: Administrator searches with spacing between words

- **WHEN** an authorised administrator searches using words separated by
  whitespace
- **THEN** matching member data is returned whether it stores whitespace
  between those words or not

#### Scenario: Administrator filters members by role

- **WHEN** an authorised administrator selects a moderation role
- **THEN** the first page of members with that role is displayed

#### Scenario: Administrator sorts member search results

- **WHEN** an authorised administrator selects a member-table column header
- **THEN** the displayed member collection is sorted by that column on the server
- **AND** selecting the active header again reverses the sort direction

#### Scenario: Administrator opens another member-list page

- **WHEN** an authorised administrator selects a subsequent or previous page
- **THEN** the system displays that page of the same member collection
- **AND** retains the selected sort column and direction

#### Scenario: Administrator submits an invalid search or role

- **WHEN** an authorised administrator submits an invalid member search or role
- **THEN** the system explains that the request is invalid

### Requirement: Member reports and moderation notes

The system SHALL provide authorised administrators with a member report and
allow them to record moderation notes about that member.

#### Scenario: Administrator opens a member report

- **WHEN** an authorised administrator opens a report for an existing member
- **THEN** the report displays the member's moderation-relevant information

#### Scenario: Administrator saves a moderation note

- **WHEN** an authorised administrator adds a note to a member report
- **THEN** the note is saved and displayed with that member's notes

#### Scenario: Administrator requests a missing or malformed member report

- **WHEN** an authorised administrator requests a report with a missing or malformed member identifier
- **THEN** the system returns a usable error response

### Requirement: Role changes and audit history

The system SHALL let authorised administrators apply permitted moderation-role
changes and review the administration audit history.

#### Scenario: Administrator changes a member's moderation role

- **WHEN** an authorised administrator applies a permitted role change
- **THEN** the member's role is updated
- **AND** the action is recorded in the audit history

#### Scenario: Administrator requests an impermissible role change

- **WHEN** an authorised administrator requests a role change that is not permitted
- **THEN** the system rejects the request

### Requirement: Conversation and reference inspection

The system SHALL let authorised administrators inspect message threads,
messages between identified members, and reference threads for moderation.

#### Scenario: Administrator inspects messages between members

- **WHEN** an authorised administrator requests messages between two valid members
- **THEN** the system displays the messages available for moderation, including shadow-hidden content

#### Scenario: Administrator inspects member threads

- **WHEN** an authorised administrator queries threads by a member identifier or username
- **THEN** the system returns the matching threads

#### Scenario: Administrator submits an invalid member identifier for inspection

- **WHEN** an authorised administrator submits a malformed member identifier
- **THEN** the system rejects the request with an error response

### Requirement: Readable moderation context

The system SHALL present member search results, reports, message inspection,
and reference threads in a form that lets authorised administrators move
between related members and activity.

#### Scenario: Administrator inspects a member's activity

- **WHEN** an authorised administrator opens a member report or search result
- **THEN** the system presents the member's relevant profile, role, contact,
  offer, and moderation information with links to related administration views

#### Scenario: Administrator searches message participants by username

- **WHEN** an authorised administrator supplies member usernames to message inspection
- **THEN** the system resolves the members and displays their conversation context
- **AND** any related reference-thread votes are shown

### Requirement: Administration operational views

The system SHALL provide authorised administrators with administration views
for audit history, newsletter subscribers, acquisition stories, and acquisition
analysis. The acquisition-stories view SHALL identify members with profile
pictures and public-profile links, show their circle participation and available
location context, and allow the available columns to be sorted.

#### Scenario: Administrator opens acquisition stories

- **WHEN** an authorised administrator opens the acquisition-stories view
- **THEN** each story identifies its member with a profile picture and public-profile link
- **AND** shows the number of circles that member has joined
- **AND** shows available living and origin locations
- **AND** shows the latest hosting-offer location using fuzzy coordinates when available
- **AND** the administrator can sort the rows by date, member, circle count, or story

#### Scenario: Administrator opens an available operational view

- **WHEN** an authorised administrator opens an available operational view
- **THEN** the requested view displays its available data

#### Scenario: Administrator requests unavailable newsletter subscriber data

- **WHEN** an authorised administrator requests newsletter subscriber data while the subscriber API is unavailable
- **THEN** the system fails safely without exposing subscriber data

### Requirement: Current IP address moderation context

The system SHALL retain only the current client IP address observed during a
member's authenticated activity. It SHALL make that address available only to
authorised administrators in member-search results and member reports.

#### Scenario: Member performs authenticated activity

- **WHEN** a member performs authenticated activity from a client IP address
- **THEN** the system records the member's current client IP address
- **AND** replaces any previously stored IP address for that member

#### Scenario: Administrator views a member

- **WHEN** an authorised administrator views a member in a search result or
  member report
- **THEN** the system displays the member's current stored IP address when one
  is available

#### Scenario: Administrator follows an IP address

- **WHEN** an authorised administrator selects a displayed IP address
- **THEN** the system displays members whose current stored IP address exactly
  matches that address

#### Scenario: Regular member requests an IP-address lookup

- **WHEN** a regular member requests the administrator IP-address lookup
- **THEN** the system denies access
