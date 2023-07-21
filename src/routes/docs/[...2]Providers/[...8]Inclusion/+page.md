---	
title: Transaction Inclusion List	
description: builder-boost instances have a toggle option to activate Transaction Inclusion Lists for Users to gain early visibility into their inclusion.
---	

# {$frontmatter.title}	

{$frontmatter.description}	
:::admonition type="note"
this is an experimental feature we're collecting data on during the pilot phase
:::


```javascript	
{	
"builder":"0xaa1488eae4b06a1fff840a2b6db167afc520758dc2c8af0dfb57037954df3431b747e2f900fe8805f05d635e9a29717b",	
"number":17671850,	
...	
"personal_transactions":[0xtxnhash1, 0xtxnhash2,...],	

}	
```	

To activate it, perform the following:	

### Option 1: Set Environment Variable	
Set the envrionment variable **INCLUSION_LIST=true**	

### Option 2: Add Parameter	
```bash 	
./boost --inclusionlist true	
```
