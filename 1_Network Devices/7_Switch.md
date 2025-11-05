# Switch

Now, both hub and bridge have limitations: - A **hub** causes data
collisions (broadcasts everywhere). - A **bridge** can only connect two
network segments.

So, we use a **switch** --- which is like a **smart combination** of
both.

## Switch Features:

-   Multiple ports (like a hub).
-   Learns which host is connected to which port (like a bridge).
-   Facilitates communication **within the same network** efficiently.

When devices are connected to a switch, they're part of the **same
network** --- meaning they share the same IP address space.

## Example:

If Host1 wants to send data to Host3 (but not Host2), the switch sends
data **only** to Host3 --- not to everyone.

Think of it like this: Within Denmark, if someone in Copenhagen wants to
send data to another city, the switch ensures it goes directly there ---
not to every city in Denmark.
