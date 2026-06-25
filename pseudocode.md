```
FUNCTION f(a):
	IF a==0:
		RETURN 1
	ELSE:
		RETURN a*CALL f(a-1)
FUNCTION g(n):
	SET result TO 0
	FOR i FROM 1 TO n:
		INCREASE result BY CALL f(i)
	RETURN result
SET ouput TO CALL g(4)
```