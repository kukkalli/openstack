## Setup ssh keys
### Create ecdsa keys
Link to [ssh-keygen](https://www.ssh.com/ssh/keygen/#choosing-an-algorithm-and-key-size) source.

- ecdsa - a new Digital Signature Algorithm standarized by the US government, using elliptic curves. This is probably a good algorithm for current applications. Only three key sizes are supported: 256, 384, and 521 (sic!) bits. We would recommend always using it with 521 bits, since the keys are still small and probably more secure than the smaller keys (even though they should be safe as well). Most SSH clients now support this algorithm.

```
ssh-keygen -t ecdsa -b 521
```

#### Edit ```.ssh/authorized_keys``` and add the generated keys from each server
```
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBAGxlZsduAGeKqz3UhzHeXiJOsRlBQTZIyOxA0DrXso9ncDveooDqUr+Xw5XZx44nHFNjWocoQowDdaA8jj0DYEs9wF5ELGj/rm4n6a1b6tXVAlb3Vojb5C0mZfx2gUA6i5GNnNXONRttaW53XeOoD/VDM9tlgBnpa04bBQ1naTiLbQsQg== os@controller
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBAFJ/TSfJegktNbVbCF2L1hte8qfDtgk/zArlNq4vgEAKRePSEYnoFldlGVn5zDqnvLP2xy6WrcFUjO2TOeTnmqQ1gEzcBOjUXeYdA7LO1J8yARvvAMOk4IiuVTvGUdCIW8uDpXwfqCxqeKbSudo3LVLgt/ZcRg1QENyRLP/zqixIJoEsA== os@compute01
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBACnHtnIvTXuTG2I5ngNXKUYu/h7izkEGPmfZpqIeuXcQIY0miX7k+9snBvPXKuxp5nYspOZuOzsHs4JEE3l/+ftcgHvF7w3SD5CtdTfGMhUwGHtcpWtKfj18+FiDwh9wK4m6exBChpfBTU1q14LPZBR7xg9KZULWGddugmffUK1SMoWdg== os@compute02
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBAG6PCMQcMNvSA4yRmHETOYdj60fsJo4n8FOBKmlw2fJR7xWMND0DQWTVvPssv3bw1iKn5zLbx4aeVd7idKT00HsjwB4mX1/+UBVUeP/21tp50J3XsG5Pdwz4JL6LeRWvurKoU66bpBR5u0Iuo9VrJlHfn3GbCiHzke7uUt3QBmBWkxroQ== sdn@sdnc
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBACU7DStTMa4kHmnZ6kiNkQHrW4CYW9kOKkR8xQa3yBvPDG0IYv0MuUJg2lY5TfdhmXNWELPYHlZxieOC60HTD/vzACoV3268mlJNYGE+ju4iQq+QXaUSwog4YkQs4aDCpylyDRJYWFe8YP97/xFOzR5P5bxCYcJZQLlwWa/+294kW29hQ== hanif@openstack
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBAEBCbxZcyGw+PD3S/HoPx/WKhfGOz4Mos3OGQ4Q2rvh7UpNBE4UVp/xOBcFoL0WveHI+WskQV0jKa7TnErjVwEsOAAX6O4DxaskATGq6XioPv2XmRGKb5UZ28NUCE+VLhUvnFLLn2IMiCSiNzCU8hX0rjsU6/hHjDyV01Iahq2gAY6E7Q== hanif@openstack
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBAHLT0AS5MHwwJ6hX1Up5stfz361+IWA/8/MhZBH+mYA32h/Bp5hSWkQDXow4aDiHRlxVV1WLlHHup+GPBBA9XLTRwHP8gAjbP5EM4EVxR9EbDh5Hz13xcN0/n9J9rasefHS8UgTJUgRrWeNRCSAhkbNfDfSeQzk8NWlzhiwwCIUacKnzg== hanif@kukkalli
ecdsa-sha2-nistp521 AAAAE2VjZHNhLXNoYTItbmlzdHA1MjEAAAAIbmlzdHA1MjEAAACFBAH8jfqp8rxn/ml0SLleEsoyAvwdlPI3kvv9xUW9jIS9reg+M8f9Ep79QfvwYIddr15tekYHDs3hZd2qK/oaQ06AmwCT5elhGGWvBQcrcPGs6JqnuO6qfwCxipWtdQkbOF3f7/8sxPpKwBQ9A/c4u8Q+jo/PtL2qAlw1yUcKc3FZsCUOJw== kukkalli@DESKTOP-9V82GNT
```

[Home](../README.md#initial-setup)
[Next](../environment-setup/host-networking.md#host-networking)