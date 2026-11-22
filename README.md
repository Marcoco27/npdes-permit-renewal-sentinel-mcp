# NPDES Permit Renewal Sentinel

When must the renewal application of this US water discharge permit be sent, and does that date bind?

No facility may discharge a pollutant into the waters of the United States without a permit in force (33 U.S.C. 1311(a)). An NPDES permit lasts at most five years, and 40 CFR 122.21(d) asks for the renewal application 180 days before it expires. An EPA page says a permit can be administratively extended if the facility reapplies more than 180 days before it expires, one day earlier: the two texts differ by one day, and the service returns both dates.

For 1 to 10 NPDES permit numbers, this MCP server and HTTP API read the current version of each permit in the weekly ICIS-NPDES national data set published by EPA (ECHO data downloads) and return, for each individual permit:

- the kind of record, the status as published by ECHO, the kind of issuing organisation and the kind of facility;
- the effective, expiration and termination dates as on file;
- `application_due`, the safe deadline for the renewal application (181 days before expiration), and `application_due_per_regulation`, the date of the regulation (180 days);
- `later_reading_next_working_day`, for permits issued by EPA only: the regulation date moved to the next federal working day, marked as not guaranteed;
- `due_is_binding`: true only for an individual permit issued by EPA. For a permit issued by a State, the date is the federal default and the State's own rule governs;
- the days left or past, counted in the time zone of the State named by the permit number;
- once the term has ended, the status as published by ECHO (for example administratively continued), repeated as published and never read as a legal finding. When the term ended after the date of the weekly table, the service concludes nothing;
- `term_exceeds_five_years`, when the dates on file show a term the law does not allow.

Coverage under a general permit is told apart and not computed. Dates are never moved for a weekend or a holiday, except in the one reading marked as not guaranteed. The permit itself may write its own date for the application, and EPA may allow a later one: the text of the permit prevails. No facility name, no permit holder name and no address is read or returned.

- MCP (streamable HTTP): `https://npdes.amacunia.fr/mcp`, tools `npdes_permit_lookup` (paid, 0.01 USDC per call via x402 on Base) and `npdes_permit_example` (free).
- HTTP: `GET https://npdes.amacunia.fr/v1/npdes-permit?permits=NH0100013` (x402, 0.01 USDC; free trial: 3 calls per day).
- Free sample: `https://npdes.amacunia.fr/v1/exemple`.
- Machine-readable: `/llms.txt`, `/.well-known/x402`, `/rules`, `/terms`.
- Paying client example: [`examples/pay_mcp.py`](examples/pay_mcp.py).

No account, no API key.

## Data

ICIS-NPDES national data set, published every week by the U.S. Environmental Protection Agency (ECHO data downloads). U.S. Government work, public domain. Only eight fields of the current version of each permit are read: permit number, kind of record, status, kind of issuing organisation, kind of facility, effective, expiration and termination dates. Source: U.S. EPA, ICIS-NPDES national data set (ECHO).

## Disclaimer

Information only, not legal advice; it does not replace an attorney. Not affiliated with or endorsed by the U.S. Environmental Protection Agency (EPA), any State agency or any U.S. government agency. Not a consumer report under the FCRA: do not use it to decide on employment, credit or insurance. The table is weekly: a permit renewed or reissued after its date does not show. The official EPA and State records, and the permit itself, are authoritative. Terms of use: https://npdes.amacunia.fr/terms. Contact: contact@amacunia.fr
