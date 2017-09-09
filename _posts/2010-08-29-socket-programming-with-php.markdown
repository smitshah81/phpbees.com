---
title: Socket Programming With Php
date: 2010-08-29 12:38:00 +05:30
categories:
- php
tags:
- Socket Programming
author: Smit Shah
---

To understand Socket Programming(SP) you have to understand Network Communication fundamental first and I assume that you already read it. :)

Here's a simple script for sending messages back and forth between a server and client.  At this point, the code is fairly rough because once it enters the while loop, it doesn't stop but it can be modified and fixed.(TCP/IP) Example.

##### The Server

`error_reporting(E_ALL);
$address = "127.0.0.1";
$port = "10000";
$mysock = socket_create(AF_INET, SOCK_STREAM, SOL_TCP);
socket_bind($mysock, $address, $port);
socket_listen($mysock, 5);
$client = socket_accept($mysock);
echo "Server started, accepting connections...\n";
$i = 0;
while (true == true)
{
$i++;
echo "Sending $i to client.\n";
socket_write($client, $i, strlen($i));
$input = socket_read($client, 2048);
echo "Response from client is: $input\n";
sleep(5);
}
echo "Closing sockets...";
socket_close($client);
socket_close($mysock);`

##### The Client


`error_reporting(E_ALL);
$address = "127.0.0.1";
$port = 10000;
$socket = socket_create(AF_INET, SOCK_STREAM, SOL_TCP);
if ($socket === false) {
echo "socket_create() failed: reason: " . socket_strerror(socket_last_error()) . "\n";
} else {
echo "socket successfully created.\n";
}
echo "Attempting to connect to '$address' on port '$port'...";
$result = socket_connect($socket, $address, $port);
if ($result === false) {
echo "socket_connect() failed.\nReason: ($result) " . socket_strerror(socket_last_error($socket)) . "\n";
} else {
echo "successfully connected to $address.\n";
}$i = 0;
while (true == true)
{
$i++;
echo "Sending $i to server.\n";
socket_write($socket, $i, strlen($i));
$input = socket_read($socket, 2048);
echo "Response from server is: $input\n";
sleep(5);
}
echo "Closing socket...";
socket_close($socket);`
