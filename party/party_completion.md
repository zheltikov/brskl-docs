# Check the Party fields that need to be completed

Method: `GET`

Endpoint: `/api/company/v2/dashboard/party/get-fields-to-complete`

Request format:

```ts
interface Request {
    party_id: int;    // The ID of the Party to be checked
}
```

The response contains four sections, each with the fields that need to be completed:

- `account`: For the Bank info
- `doc2`: For personal info for an Individual
- `party`: For other info in the party
- `file`: For info about the Passport pages

The response format is as follows:

```ts
type Field = { required: true };
type OptionalField = { required: false };

interface Response {
    account: {
        account?: Field,
        bank_bic?: Field,
        bank_name_full?: Field,
    };
    doc2: {
        authority_code?: Field,
        authority_name?: Field,
        birth_date?: Field,
        birth_place?: Field,
        citizenship?: Field,
        issue_date?: Field,
        name_first?: Field,
        name_last?: Field,
        name_middle?: OptionalField,
        number?: Field,
        registration_place?: Field,
        series?: Field,
    };
    party: {
        actual_address?: Field,
        capital?: Field,
        inn?: Field,
        kpp?: Field,
        manager_name?: Field,
        name_full_with_opf?: Field,
        name_short_with_opf?: Field,
        non_secure3d?: Field,
        ogrn?: Field,
        okopf_code?: Field,
        okved_code?: Field,
        phones?: Field,
        secure3d?: Field,
        swift_bic?: Field,
        taxation_system?: Field,
    };
    file: {
        'physical-person-passport-page-1'?: Field,
        'physical-person-passport-page-2'?: Field,
    };
}
```
