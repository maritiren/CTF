# Hash Length Extension
In hash length extension, a vulnerability in SHA1 and SHA2
allows us to add some data at the end of a hash without knowing 
the secret. 

You may recognise this type of task if the web server does 
hash validation like this:
```
hash = sha512(secret + data)
```
Usually, you need access to a page or something on a web server, 
and this web server checks a hash and the data from the header 
or cookies, or as any kind of input in the request you make. 
The server use the input data to make a new hash with the secret,
and then checks the new hash towards the input hash. Somewhat
like the following code:

```python
def validate_input_hash(secret, input_data, input_hash):
  new_hash = sha512(secret + input_data)
  
  if new_hash == hash:
    give_flag()
    
  return not_flag
```

An important part is to know the length of the secret. If this 
is unknown, you may try to do a bruteforce attack with different
lengths.

## Examples
#### Hash and Data in request header
The following script let you bruteforce the length of the secret 
calling the server `i` amount of times using curl and outputs 
the response. If the response is right, the flag should
be returned. At least a new page getting you further in the task:

```python
import subprocess

for i in range(1,20):
    hash_extender_input = "./hash_extender --data 'known data from server'"
    hash_extender_input += "-s e20e4f2703891bfa49c2a84dbe7ce125d716be28d44e82209db28fb97e0718fc38ea79947189d5ddcb8c83a1de443cae4bd95077a1b0abe6b83b18767c1f8584"
    hash_extender_input += "--append 'data to append to get access' -l {} --out-data-format=html -f sha512".format(i)
    hash_extender_output = subprocess.check_output(hash_extender_input, shell=True)

    # These lines takes the output of the hash_extender tool
    # and fetches the hash and data to send to the server
    new_hash_string = hash_extender_output.split("\n")[2]
    new_data_string = hash_extender_output.split("\n")[3]
    new_data = new_data_string.strip(" ").split(":")[1]
    new_hash = new_hash_string.strip(" ").split(":")[1]

    response = subprocess.check_output('curl -H "data:{}" -H "hash:{}" https://url.here.ctf/admin'.format(new_data, new_hash), shell=True)
    print response
```

## Tools:
* https://github.com/iagox86/hash_extender 

## Read more: 
* https://blog.skullsecurity.org/2012/everything-you-need-to-know-about-hash-length-extension-attacks
* Writeup from PlaidCTF: https://blog.skullsecurity.org/2014/plaidctf-web-150-mtpox-hash-extension-attack
