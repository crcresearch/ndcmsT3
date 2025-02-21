# Maintenance

The CRC performs major renovations of physical infrastructure and software / module changes during two scheduled outages within each calendar year. These outages always occur first in January during the break between Fall and Spring semesters, and during the weekend of undergraduate graduation.

All systems are unavailable during these maintenance periods including compute hosts. All UGE / HTCondor jobs are removed from queues and any jobs still running once a biannual maintenance period begins are removed.

------------------------------------------------------------------------

## Typical Changes

- `RHEL` OS updates
  - system `GCC` compiler updates
  - system `Python` updates
- Server firmware / bios patches
- Core software updates / patches including:
  - module command
  - `UGE`
  - `HTCondor`

------------------------------------------------------------------------

## Module Changes

Within each biannual maintenance, the collection of software modules follows a planned upgrade model. New modules may be added, modules added to the deprecated list in a previous maintenance period will be removed, and older modules will be added to the deprecated list to be removed during the next maintenance. The lifetime of a specific module depends on several factors such as the development cycle of the software, the security risk of the software, usage of the module, and other factors.

The typical life of module XYZ is below:

    Created April 2019 --> Deprecated during January 2021 Maintenance --> Removed during May 2021 Maintenance

More information on the module system can be found on the `modules` page.

------------------------------------------------------------------------

## Reoccuring Maintenance

The CRC reserves the right to perform maintenance and security patching on machines each Sunday morning from 2:00 am - 10:00 am EST. During this time front end machines may be unavailable.

------------------------------------------------------------------------

## Past Maintenance Periods

- `See the past maintenances <past-maintenances>`
