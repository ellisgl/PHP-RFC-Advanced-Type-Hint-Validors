# PHP RFC: Addition of advanced type hint validators.

## Introduction:
PHP has basic data type hints.

The purpose of the RFC is to propose extending type hints with chainable advance validators.

* Pros:
    * Can restrict a data type acceptance criteria and remove the need for some data validation libraries.

* Cons:
    * Chained validators could lower code readability.

## Proposed usage / Syntax:
    int->range(0, 5)                     $a;
    int->gt(0)                           $b;
    int->lt(5)                           $c;
    string->len(0, 5)                    $d;
    string->len(4)                       $e;
    string->numeric                      $f;
    string->numeric->regex(/^\d\d.\d/$)  $g;
    string->regex(/^\w+\d\d$/)           $h;
    string->ip                           $i;
    string->ip6                          $j;
    string->email                        $k;
    float->range[0.00-0.10]              $l;
    ?int->range(0, 5)                    $m;
    ?int->gt(0)                          $n;
    ?int->lt(5)                          $o;
    ?string->len(0, 5)                   $p;
    ?string->len(4)                      $q;
    ?string->numeric                     $r;
    ?string->numeric->regex(/^\d\d.\d/$) $s;
    ?string->regex(/^\w+\d\d$/)          $t;
    ?string->ip                          $u;
    ?string->ip6                         $v;
    ?string->email                       $w;
    ?float->range[0.00-0.10]             $x;
    
    function xyz(int->range(0-5) $a)
    {
        // ...
    }

- OR? -
    
    int::range(0, 5)                     $a;
    int::gt(0)                           $b;
    int::lt(5)                           $c;
    string::len(0, 5)                    $d;
    string::len(4)                       $e;
    string::numeric                      $f;
    string::numeric->regex(/^\d\d.\d$/)  $g;
    string::regex(/^\w+\d\d$/)           $h;
    string::ip                           $i;
    string::ip6                          $j;
    string::email                        $k;
    float::range(0.00, 0.10)             $l;
    ?int::range(0, 5)                    $m;
    ?int::gt(0)                          $n;
    ?int::lt(5)                          $o;
    ?string::len(0, 5)                   $p;
    ?string::len(4)                      $q;
    ?string::numeric                     $r;
    ?string::numeric->regex(/^\d\d.\d$/) $s;
    ?string::regex(/^\w+\d\d$/)          $t;
    ?string::ip                          $u;
    ?string::ip6                         $v;
    ?string::email                       $w;
    ?float::range(0.00, 0.10)            $x;

    function xyz(int::range(0-5) $a)
    {
        // ...
    }
    
## Warning / Errors / Exceptions:
    $a = 10;
    // Should: throw new TypeError("'10' is not in range(0, 5).")
    
    $b = -1;
    // Should: throw new TypeError("'-1' is not greater than 0.")
    
    $c = 6;
    // Should: throw new TypeError("'6' is not less than 5.")
    
    $d = 'This is a string';
    // Should: throw new TypeError("'This is a string' is out of range len(0, 5).")
    
    $e = 'abc';
    // Should: throw new TypeError("'This is a string' is out of range len(4).")
    
    $f = '1x';
    // Should: throw new TypeError("'1z' is not numeric.")
    
    $g = '1y';
    // Should: throw new TypeError("'1y' is not numeric.")
    
    $g = '100.0';
    // Should: throw new TypeError("'100.0' does not match regex(/^\d\d.\d/$).")
    
    $h = '11';
    // Should: throw new TypeError("'11' does not match regex(/^\w+\d\d$/).")
    
    $i = '333.255.255.0';
    // Should: throw new TypeError("'333.255.255.0' is not a valid IP address.")
    
    $j = '2001:db8:1234::xxxx';
    // Should: throw new TypeError("'2001:db8:1234::xxxx' is not a valid IPv6 address.")
    
    $k = 'xxx.com';
    // Should: throw new TypeError("'xxx.com' is not a valid email address.")
    
    $l = 0.11;
    // Should: throw new TypeError("'0.11' is not in range(0.00, 0.10).")