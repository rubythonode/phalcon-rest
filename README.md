#Phalcon REST#
[![Latest Stable Version](https://poser.pugx.org/olivierandriessen/phalcon-rest/v/stable)](https://packagist.org/packages/olivierandriessen/phalcon-rest) 
<a href="http://phalconist.com/olivierandriessen/phalcon-rest" target="_blank">
![Phalcon REST Library](http://phalconist.com/olivierandriessen/phalcon-rest/default.svg)
</a>

*A flexible library, consisting of interchangeable classes made for the modern REST API.*

 * Complex/flexible JSON formatting ([Fractal](https://github.com/thephpleague/fractal), [Build API's You Won't Hate](https://leanpub.com/build-apis-you-wont-hate))
 * Authentication Manager for managing different Account Types.
 * Session handling for ([JWT](http://jwt.io/))
 * Access control on endpoints ([Phalcon ACL](http://docs.phalconphp.com/en/latest/reference/acl.html))
 * Documentation generator ([Phalcon Annotation Reader](https://docs.phalconphp.com/en/latest/reference/annotations.html))
 * [Postman REST Client](http://getpostman.com) Collection Export generator

## Installing ##
Install using Composer. Not all dependencies are required.
````
composer install olivierandriessen/phalcon-rest
````

## Boilerplate ##
For a full implementation of the library, check out the [Boilerplate application](https://github.com/olivierandriessen/phalcon-rest-boilerplate).

## Table of Contents

1. [API Service](#api-service)
1. [Data Queries](#data-queries)

## API Service

- [API Service Setup](#api-service-setup)

###### [API Service Setup]

- Instantiate the API Service then add resources to it

    ````php
    /**
     * @description App - \PhalconRest\Api\Service
     */
    $di->setShared(AppServices::API_SERVICE, function () {
    
        $apiService = new \PhalconRest\Api\Service;
    
        $itemResource = new \PhalconRest\Api\Resource();
        $itemResource
            ->setKey('items')
            ->setModel('\Item')
            ->setTransformer('\ItemTransformer');
    
        $apiService->addResource($itemResource);
    
        return $apiService;
    });
    ````

## Data Queries

- [URL Query Commands](#url-query-commands)
- [Using phqlQueryParser](#using-phqlqueryparser)

###### [URL Query Commands]

- **fields**
    The `fields` command allows you to include just the fields you need.

    > ?fields=title,author

- **offset**
    The `offset` command allows you to exclude a given number of the first objects returned from a query. this is commonly used for paging, along with `limit`.

    > ?offset=10

- **limit**
    The `limit` command allows you to limit the amount of objects that are returned from a query. This is commonly used for paging, along with `offset`.

    > ?limit=100

- **having** 

    - author `Is Equal` 'Jake' `AND` likes `Is Equal` 10

        > ?having={"author":"Jake","likes":10}

- **where**

    - author `Is Like` Jake

        > ?where={"author":{"l":"Jake"}}

    - author `Is Equal` Jake

        > ?where={"author":{"e":"Jake"}} 

    - author `Is Not Equal` Jake

        > ?where={"author":{"ne":"Jake"}} 

    - likes `Is Greater Than` 10

        > ?where={"likes":{"gt":10}} 

    - likes `Is Less Than` 10

        > ?where={"likes":{"lt":10}} 

    - likes `Is Greater Than Or Equal To` 10

        > ?where={"likes":{"gte":10}} 

    - likes `Is Less Than Or Equal To` 10

        > ?where={"likes":{"lte":10}} 

- **or**
    The `or` command allows you to specify multiple queries for an object to match in an array.

    > ?or=[{"author":{"e":"Jake"}},{author:{"e":"Alex"}}]

- **in**
    The `in` command allows you to specify an array of possible matches.

    > ?in={"author":["Jake", "Billy"]}

- **sort**

    The `sort` command allows you to order your results by the value of a property. The value can be 1 for ascending sort (lowest first; A-Z, 0-10) or -1 for descending (highest first; Z-A, 10-0).

    - Descending

        > ?sort={"likes":-1}

    - Ascending

        > ?sort={"likes":1}
        
###### [Using phqlQueryParser]

- `query` is a global object that contains a formatted query based on the urls query params.
- Use the `phqlQueryParser` to automatically apply all the url query commands to a new [phqlBuilder](https://docs.phalconphp.com/en/latest/api/Phalcon_Mvc_Model_Query_Builder.html) instance.

    ````php
    /** @var \PhalconRest\Data\Query $query */
    $query = $this->get(AppServices::QUERY);

    /** @var \PhalconRest\Data\Query\Parser\Phql $phqlQueryParser */
    $phqlQueryParser = $this->get(AppServices::PHQL_QUERY_PARSER);

    /** @var \Phalcon\Mvc\Model\Query\Builder $phqlBuilder */
    $phqlBuilder = $phqlQueryParser->fromQuery($query);

    // Resultset
    $results = $phqlBuilder->getQuery()->execute();
    ````

## External links ##
Blog on [Getting Started with Phalcon REST](http://olivierandriessen.com/getting-started-with-phalcon-rest/)

## Contributing ##
Please file issues under GitHub, or submit a pull request if you'd like to directly contribute.

## Changelog ##

*1.1.1* Add Resources, QueryParsers
*1.0.1* Update README
*1.0.0* Stable release with several improvements
*0.0.4* Release unfinished but stable version  
*0.0.3* Added Auth/Account/Email, changed user registration flow  
*0.0.2* Major changes  
*0.0.1* Major changes  

###Todo###
* ~~Convert indentation to spaces~~
* DocBlocks
* PSR-2 coding standard
