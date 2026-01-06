ReadMe

This code can be used to calculate (local) ideal class monoids of function fields
and the product of local gekeler ratios for an isogenyclass of Drinfeld modules.
For the theoretical background see: voeg link artikel in
The code can be copy-pasted into a local version of the computer algebra system Magma
or into the online Magma calculator: https://magma.maths.usyd.edu.au/calc/ .

The probably most important functions are:

ICMp(R,p)
RngFunOrd,RngUPolElm -> [RngFunOrdIdl] 
Given an order R = A[x]/f(x) with f irreducible and p a prime of A, calculate 
the ideal class monoid of R_p. It is not know if the algorithm gives the correct
answer if R is not of this form. 

ProductOfLocalGekelerRatios(poly)
RngUPolElm[RngUPol[FldFin]] -> a number 
Returns the product of all local gekeler ratios of poly, i.e. \prod_p v_p(poly)

MinimalPolynomialOfFrobenius(A,phi)
given a polynomial ring A = F_q[T] and a Drinfeld Module A->F_{q^n}{tau}
mapping T to phi, return the minimal polynomial of the Frobenius endomorphism

SingularPrimes(A,poly)
RngUPol,RngUPolElm -> [RngUPolElm] 
Calculates the singular primes of the order A[x]/poly(x)  

OrdersInBetween(Rr,Oo)
RngFunOrd, RngFunOrd -> [RngFunOrd]
Calculates all orders in between Rr and Oo, where Rr \subset Oo

LocalWeakEquiv(I,J,p)
FracIdeal[RngFunOrd],FracIdeal[RngFunOrd], RngUPolElm -> Boolean 
Calculates if the fractional ideals I and J are locally at p weak equivalent

pOverorders(R,p)
RngUPol, RngFunOrd,RingUPolElm -> [RngFunOrd]
For an A-order R we calculate the p-overorders over R 
If R = A[x]/f(x) with f irreducible, then this function also gives representants
for the overorders of R_p

//Examples//////////////////////////////////////////////////////////////////////
//The part below can be copied into Magma below the source code, to run some examples.

//We first set up the rings
q:= 3;
Fq:= FiniteField(q);
Fqn<alpha> := ext<Fq|2>;
A<T>:= PolynomialRing(Fq);
F<T>:= FieldOfFractions(A);
Fx<x> := PolynomialRing(F);
ATx<x>:= PolynomialRing(A);
ktau<tau>:= TwistedPolynomials(Fqn);

//We take a phi and calculate its minimal polynomial of the frobenius
//If this has the same degree as the rank of phi, we know that phi has a commutative endomorphism ring
phi := tau^3+ tau^2 + tau;
minpoly := MinimalPolynomialOfFrobenius(A,phi);
print("The minimal polynomial is:");
print(minpoly);
print("The degree of the minimal polynomial equals the rank of phi:");
print(Degree(minpoly) eq Degree(phi));

//We then calculate the singular primes
SingPrimes := SingularPrimes(A,minpoly);
print("The singular primes are:");
for p in SingPrimes do
    print(p);
end for;

//We can calculate the local ideal class monoid at the singular prime(s)
//and check that the first two ideals are not weak equivalent
K := FunctionField(minpoly);
R := EquationOrderFinite(K);
for p in SingPrimes do
    classes := ICMp(R,p);
    print("The local ICM is:");
    print(classes); 
    print("and the first two classes are locally weak equivalent:");
    print(LocalWeakEquiv(classes[1],classes[2],p));
end for;

//Similar we can calculate the overorders at the singular prime(s)
for p in SingPrimes do
    print("The local overorders are:");
    print(pOverorders(R,p));
end for;

//Last we can calculate the product of the local Gekeler ratios
print("The product of the local Gekeler ratios is:");
print(ProductOfLocalGekelerRatios(minpoly));
