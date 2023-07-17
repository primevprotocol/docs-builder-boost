---
title: Transaction Inclusion List
description: Boost instances have a toggle option to activate Transaction Inclusion Lists for Searchers to Consume.

---

# {$frontmatter.title}

{$frontmatter.description}

```javascript
{
"builder":"0xaa1488eae4b06a1fff840a2b6db167afc520758dc2c8af0dfb57037954df3431b747e2f900fe8805f05d635e9a29717b",
"number":17671850,
...
"personal_transactions":[0xtxnhash1, 0xtxnhash2,...],

}
```

To activate it, you can do the following:

### Option 1: Environment Variable
Set the envrionment variable **INCLUSION_LIST=true**

### Option 2: Parameter
```bash 
./boost --inclusionlist true
```