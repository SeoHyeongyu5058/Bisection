# Bisection

**개요**<br>
* [bisection( )]()<br>
* [newtonRaphson( )]()<br>
* [secant( )]()<br>

<hr>

## bisection( )<br>

```c
double bisection(double func(double x), double a, double b, double tol);
```
>Parameter<br>
**func(double x)** : x를 변수로 받는 System func  <br>
**a** : 초기 첫번째점 <br>
**b** : 초기 두번째점 <br>
**tol** : tollernce 

1. 함수 f(x)는 연속이며 [ a, b ]내에서 미분 가능이어야 한다. 
2. f(a) * f(b) < 0 인 경우에만 사용가능하다. 
3. myNP.h 안에 선언되어 있다.
4. 다음과 같은 원리를 기반으로 작성되었다.<br>

* Estimate the next solution as

$$X_N(1)={{a+b}\over 2}$$

* Check whether X_T is closer to a or b:

$$f(a)f(X_N)<0$$
$$a<X_T<X_N(k)$$
$$X_N(k+1)={{a+X_N(k)}\over 2}$$

$$f(a)f(X_N)>0$$
$$b>X_T>X_N(k)$$
$$X_N(k+1)={{b+X_N(k)}\over 2}$$

* Check the tolerance or rel.error

$$Tol_{f(x)}(k)=|f(x_N(k))|=\epsilon$$

<hr>

## Example <br>
```c++
int main()
{
    double sol;
	double x0 = 10;
	double x1 = 9;
	double tol = 1.0e-6;

    sol = bisection(func, x0, x1, tol);

    printf("Final Solution: %f \t", sol);
}

double func(double x)
{
	double F = 0;
	F = 20 * tan(x*PI/180) - 9.8 * power(20, 2) / ((2*power(17, 2))*power(cos(x*PI/180), 2)) - 2;
	
	return F;
}
```

## Output <br>
```c
Final Solution: 9.000000
```

## Warning
>func(a) * func(b) > 0

## Error Handling
```c
if (func(a) * func(b) > 0)
{
	printf("No solution exists!\n");
	printf("Enter the correct inital interval\n");
}
```

<br>

## newtonRaphson( )<br>

```c
double newtonRaphson(double func(double x), double dfunc(double x), double x0, double tol);
```
>Parameter<br>
**func(double x)** : x를 변수로 받는 System func  <br>
**dfunc(double x)** : Sytem func 미분한 함수 <br>
**x0** : 시작 변수 <br>
**tol** : tollernce 

1. 함수 f(x)는 연속이며 미분 가능이어야 한다. 
2. 해와 가까운 점에서 시작되어야 한다.
3. myNP.h 안에 선언되어 있다.
4. 다음과 같은 원리를 기반으로 작성되었다.<br>

$$f^{'}(x_k)={{l(x_{k+1})-f(x_k)}\over {x_{k+1}-x_k}}$$

$$x_{k+1}=x_k-{{f(x_k)}\over f^{'}(x_k)}$$

* Repeat until the estimation error is smaller than a specified value

$$\vert{{x_{k+1}-x_k}\over x_k }\vert\leq\epsilon$$

<hr>

## Example <br>
```c++
int main()
{
    double sol;
    double x0 = 10;
    double x1 = 9;
    double tol = 1.0e-6;

    printf("Newton-Raphson Method Result:\n");
    sol = newtonRaphson(func, dfunc, x0, tol);

    printf("Final Solution: %f \t", sol);
}

double func(double x)
{
	double F = 0;
	F = 20 * tan(x*PI/180) - 9.8 * power(20, 2) / ((2*power(17, 2))*power(cos(x*PI/180), 2)) - 2;
	
	return F;
}

double dfunc(double x)
{
	double dF = 0;
	dF = 20 / power(cos(x*PI/180), 2) - 9.8 * power(20, 2) * sin(x*PI/180) / (power(17, 2) * power(cos(x*PI/180), 3));
	return dF;
}
```

## Output <br>
```c
Final Solution: 28.227848
```

## Warning
>dfunc is not zero

## Error Handling
```c
if (dfunc(x) == 0)
{
	printf("[ERROR] dF == 0 !!\n");
    break;	
}
```

<br>

## secant( )<br>

```c
double secant(double func(double x), double dfunc(double x), double x0, double x1, double tol);
```
>Parameter<br>
**func(double x)** : x를 변수로 받는 System func  <br>
**dfunc(double x)** : Sytem func 미분한 함수 <br>
**x0** : 시작 변수 <br>
**x1** : 다음 변수
**tol** : tollernce 

1. 함수 f(x)는 연속이며 미분 가능이어야 한다. 
2. Newton method와 유사하지만 두개의 점을 이용한다.
3. Bisection과 달리 부호가 같아도 근을 찾을 수 있따.
4. 다음과 같은 원리를 기반으로 작성되었다.<br>

* Choose two initial values x0, x1

* Estimate the next solution x2

$$x_2=x_1-{{x_1-x_0}\over {f(x_1)-f(x_0)}}f(x_1)$$

$$x_{k+1}=x_k-{{x_k-x_{k-1}}\over {f(x_k)-f(x_{k-1})}}f(x_k)$$

> Note:
> The convergence can fail if the secant slope is similar to 0 near the root. It can jump a large amount.

<hr>

## Example <br>
```c++
int main()
{
    double sol;
    double x0 = 10;
    double x1 = 9;
    double tol = 1.0e-6;

    printf("Secont Method Result:\n");
    sol = secant(func, dfunc, x0, x1, tol);
}

double func(double x)
{
	double F = 0;
	F = 20 * tan(x*PI/180) - 9.8 * power(20, 2) / ((2*power(17, 2))*power(cos(x*PI/180), 2)) - 2;
	
	return F;
}

double dfunc(double x)
{
	double dF = 0;
	dF = 20 / power(cos(x*PI/180), 2) - 9.8 * power(20, 2) * sin(x*PI/180) / (power(17, 2) * power(cos(x*PI/180), 3));
	return dF;
}
```

## Output <br>
```c
Iteration : 1           X(n) : 27.183174        Tolerance : 0.2995324590
Iteration : 2           X(n) : 28.176161        Tolerance : 0.0147826751
Iteration : 3           X(n) : 28.227712        Tolerance : 0.0000397857
Iteration : 4           X(n) : 28.227851        Tolerance : 0.0000000054
Final Solution: 28.227851
```

## Warning
>dfunc is not zero

## Error Handling
```c
if (dfunc(x) == 0)
{
	printf("[ERROR] dF == 0 !!\n");
    break;	
}
```

<br>
