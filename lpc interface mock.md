stty -F /dev/ttyS5 9600 raw -echo
printf '*1&1&0&0&10#\r\n' > /dev/ttyS5

stty -F /dev/ttyS5 9600 raw -echo
printf '*2&1&0&0&10#\r\n' > /dev/ttyS5

stty -F /dev/ttyS5 9600 raw -echo
printf '*2&0&0&1&10#\r\n' > /dev/ttyS5