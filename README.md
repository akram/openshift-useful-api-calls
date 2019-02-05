# Useful API Calls

## Preamble
```
export URL=$( oc whoami --show-server )
export TOKEN=$(oc whoami -t )
export HEADERS="-H \"Authorization: Bearer $TOKEN\" -H 'Accept: application/json' -H 'Content-Type: application/json' "
export CURL_OPTIONS="-k -XPOST -d@- "
```

## Create project
```
sed s/PROJECT/my-project/g project.json | curl $CURL_OPTIONS $HEADERS $URL/apis/project.openshift.io/v1/projects
```

## Create user
```
sed s/USER/my-user@sigma.fr/g user.json | curl $CURL_OPTIONS $HEADERS $URL/apis/user.openshift.io/v1/users
```

## Add manager role to user
```
PROJECT=my-project
sed s/USER/my-user@sigma.fr/g role-binding.json | sed s/ROLE/manager/g | \
    curl $CURL_OPTIONS $HEADERS $URL/apis/authorization.openshift.io/v1/namespaces/$PROJECT/rolebindings
```

## Set quota
```
PROJECT=my-project
sed s/CPU/1/g quota.json | sed s/RAM/1G/g | sed s/PVC/10/g | curl $CURL_OPTIONS $HEADERS $URL/api/v1/namespaces/$PROJECT/resourcequotas
```

## Set project labels
```
curl -k -XPATCH -d@-  -H "Authorization: Bearer $TOKEN" -H 'Accept: application/json' -H 'Content-Type: application/strategic-merge-patch+json' $URL/api/v1/namespaces/my-project < namespace-patch.json

```


