
# Bridge
To fix hub limitations, we use a **Bridge**.  
A bridge connects two network segments (usually two hubs) and has **two ports**.  
It learns which hosts are on which side and forwards data **only** where it’s needed.

## Key Feature: Learning

A bridge **learns** which hosts are on which side of the network.\
So, if Host1 (connected to Hub A) wants to send data to Host2 (also on
Hub A), the bridge knows not to send it to Hub B, which might have Host3
and Host4.

## Example:

Imagine Denmark and Sweden are connected by a bridge.\
The bridge ensures that data meant for Denmark stays in Denmark --- it
doesn't send it to Sweden unless needed.