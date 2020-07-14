---
description: Bash history can be a dangerous thing.
---

# Bash History

## Bash history is a wonderful tool.

Bash history can be a wonderful tool reminding us of all those previous commands  we may have forgotten. Gosh some shells like Fish go even further and provides auto suggestions based on previously typed commands.

## So what is the problem?

The problem is that if an unathorized person managhes to gain access to your servers he will try and elevate his permissions to other users and look at their bash history.

## What things could **they** find?

1. **Wallet Passords**

```text
./cleos.sh wallet unlock --password PW5Hxd53uNnSZR1g13F2tW55mEKzaq3h8gFCd9fvqzMmEZ5nbErqz
```

  2.  **EOS Private keys** 

```text
./cleos.sh create key --to-console 
Private key: 5HqWNReenpa6iusjfwVjqfeMjWoxDRvgB5eFzB9KBvmFgZT6fEs 
Public key: EOS6X8cXGQjvdD1Ph4zBfVTfNFfx4oChHZmwZRrUzjzexmCE4BLx4
```

## TIP and TRICKS

When logging out of your SSH shell, exit using the following command, which will delete all your history for your current session and any previous sessions. 

```text
cat /dev/null > ~/.bash_history && history -c && exit
```

You can make life even easier by creating an alias for this command.



```text
alias secureexit="cat /dev/null > ~/.bash_history && history -c && exit"
```

