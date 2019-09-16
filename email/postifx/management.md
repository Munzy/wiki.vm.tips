<!-- TITLE: Postfix Management -->
<!-- SUBTITLE: How to manage postfix email servers. -->

# Postfix Management 101

## logging

To access logging, and watch it live... an easy method is: ``` tail -f /var/log/mail.*```.

This will watch all mail related transports.

## mailq

Sometimes you need to see what is held up in the queue. To do this simply run ```mailq```.