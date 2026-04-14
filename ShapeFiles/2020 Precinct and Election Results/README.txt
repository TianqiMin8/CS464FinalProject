2020 Rhode Island precinct and election shapefile.

## RDH Date retrieval
06/10/2021

## Sources
Election results from Rhode Island Board of Elections (https://www.ri.gov/election/results/2020/general_election/data/)
Precinct shapefile from the U.S. Census Bureau's 2020 Redistricting Data Program final release.
## Fields metadata

Vote Column Label Format
------------------------
Columns reporting votes follow a standard label pattern. One example is:
G16PREDCli
The first character is G for a general election, P for a primary, S for a special, and R for a runoff.
Characters 2 and 3 are the year of the election.
Characters 4-6 represent the office type (see list below).
Character 7 represents the party of the candidate.
Characters 8-10 are the first three letters of the candidate's last name.

Office Codes

ATG - Attorney General
AUD - Auditor
COC - Corporation Commissioner
COU - City Council Member
DEL - Delegate to the U.S. House
GOV - Governor
H## - U.S. House, where ## is the district number. AL: at large.
INS - Insurance Commissioner
LTG - Lieutenant Governor
PRE - President
PSC - Public Service Commissioner
SAC - State Appeals Court (in AL: Civil Appeals)
SCC - State Court of Criminal Appeals
SOS - Secretary of State
SPI - Superintendent of Public Instruction
USS - U.S. Senate

Party Codes
D and R will always represent Democrat and Republican, respectively.
See the state-specific notes for the remaining codes used in a particular file; note that third-party candidates may appear on the ballot under different party labels in different states.

## Fields

G20PREDBID - Joseph R. Biden (Democratic Party)
G20PRERTRU - Donald J. Trump (Republican Party)
G20PRELJOR - Jo Jorgensen (Libertarian Party)
G20PREOFUE - Roque "Rocky" De La Fuente (Alliance Party)
G20PREOLAR - Gloria La Rive (Socialism and Liberation Party)
G20PREOCAR - Brian Carroll (American Solidarity Party)
G20PREOWRI - Write-in Votes

G20USSDREE - John F. Reed (Democratic Party)
G20USSRWAT - Allen R. Waters (Republican Party)
G20USSOWRI - Write-in Votes

## Processing Steps

Several precincts in Providence were aligned with the city ward shapefile.

Limited ballots were reported by township. Federal ballots were reported by congressional district. These were distributed by candidate to precincts based on their share of the precinct-level vote for the given reporting unit.