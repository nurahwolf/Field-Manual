# MariaDB

MariaDB is effectively MySQL and follows similar lifecycles.

https://endoflife.date/mariadb

#### Version History - 2025-03-18

| Version | Released | Community Support | Enterprise Support | Latest |
| --- | --- | --- | --- | --- |
| 11.7 | 12 Feb 2025 | 12 May 2025 | N/A | 11.7.2 |
| 11.4 (LTS) | 29 May 2024 | 29 May 2029 | 16 Jan 2033 | 11.4.5 |
| 10.11 (LTS) | 16 Feb 2023 | 16 Feb 2028 | 16 Feb 2028 | 10.11.11 |
| 10.6 (LTS) | 06 Jul 2021 | 06 Jul 2026 | 23 Aug 2029 | 10.6.121 |
| 10.5 (LTS) | 24 Jun 2025 | 24 Jun 2025 | 16 Jul 2025 | 10.5.28 |

Anything not in this table is EOL.

| Version | Released | Community Support | Enterprise Support | Latest |
| --- | --- | --- | --- | --- |
| 5.5 (LTS) | 11 April 2012 | 11 April 2020 | 11 April 2020 | 5.5.68 |

Releases of MariaDB are published on a regular basis. New releases are either long-term releases (LTS) or rolling releases. The support for each release depends on the release type.

Long-term release (LTS) are released yearly and supported for 3 years with bug and security fixes (was 5 years up to 11.4).

Rolling releases are released quarterly and are supported until the next release with bug and security fixes. Note that before 11.3 these were described as short-term releases and were maintained for 1 year.

Commercial support is available from MariaDB with its Enterprise offering. With an enterprise subscription, support for long-term releases is extended for 2 years, and up to 5 years with the extended release option. Note that Critical security fixes are also provided as source code releases only and on a best-effort basis for 2 additional years with the Community support.

More information is available on the MariaDB website.

You should be running one of the supported release numbers listed above in the rightmost column.

#### Fetch Latest Version

```shell
mariadbd --version
```
