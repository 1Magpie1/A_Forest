---
title: Just Testing
---

Here's an informal discussion of how I have been thinking about this equality. 

# Reformulating

The responses to questions A and B are a natural choice for a pair of bases as "Yes" and "No" are mutually exclusive responses. 
$$B_A = \{|A_y\rangle, |A_n\rangle \} \quad B_B = \{|B_y\rangle, |B_n\rangle\} $$
Since these are two subspaces of the same Hilbert space, it would make sense to write a translation matrix between them. 
$$ T_{B \rightarrow A} = \begin{pmatrix} \langle A_y | B_y \rangle &  \langle A_y | B_n \rangle\\ \langle A_n | B_y \rangle & \langle A_n | B_n \rangle \end{pmatrix} $$
$$ T_{A \rightarrow B} = T^\dagger_{B\rightarrow A} $$
Notably, this gives us the transition probabilities between questions. It is relatively simple to show that you can write these matrices as:
$$ T_{B\rightarrow A} = \begin{pmatrix} \cos(\theta) & e^{i\phi}\sin(\theta) \\ -e^{i\phi}\sin(\theta) & \cos(\theta) \end{pmatrix} $$
Since we are dealing with orthonormal vectors. In the end, we will take the magnitude of each value for our measurements, meaning that we can drop the phase term and get an orthogonal matrix. We can see this more clearly in a Bloch Sphere depiction. 

![[BlochSphere.png]]
(The figure could be better, but it illustrates the point well enough.)

We can see that the transition between $B$ and $A$ only depends on the real component, not the phase, aka it's symmetric with respect to $\phi$. This is also why we keep the negative sign in the matrix, as $B_n$ is still an antipode of $B_y$. This symmetry also gives us the QQ equality.

## Deriving the QQ

Let
$$ P_{A} = |A_y\rangle\langle A_y| \quad P_{\bar{A}} = |A_n\rangle\langle A_n| $$

With similar for the answers to $B$. 

$$ ||P_A |S\rangle||^2 + ||P_{\bar{A}} |S\rangle||^2 = 1$$
$$ ||P_B |S\rangle||^2 + ||P_{\bar{B}} |S\rangle||^2 = 1$$
$$ ||P_A |S\rangle||^2 + ||P_{\bar{A}} |S\rangle||^2 = ||P_B |S\rangle||^2 + ||P_{\bar{B}} |S\rangle||^2 $$
$$ 
\langle S|A_y \rangle \langle A_y|S\rangle 
+ 
\langle S|A_n \rangle \langle A_n|S \rangle  
=
\langle S|B_y \rangle \langle B_y|S\rangle
+
\langle S|B_n \rangle \langle B_n|S\rangle 
$$
$$ 
\alpha^2\langle S|A_y \rangle \langle A_y|S\rangle 
+ 
\alpha^2\langle S|A_n \rangle \langle A_n|S \rangle  
=
\alpha^2\langle S|B_y \rangle \langle B_y|S\rangle
+
\alpha^2\langle S|B_n \rangle \langle B_n|S\rangle 
$$
From $T_{B \rightarrow A}$, 
$$ \langle A_y| B_y\rangle = \cos(\theta) = \langle B_y| A_y \rangle
\quad
\langle A_n | B_n \rangle = \cos(\theta) = \langle B_n | A_n \rangle 
$$
Letting $\alpha = \cos(\theta)$
$$ 
\langle S|A_y \rangle \cos^{2}(\theta) \langle A_y|S\rangle 
+ 
\langle S|A_n \rangle \cos^{2}(\theta) \langle A_n|S \rangle  
=
\langle S|B_y \rangle \cos^{2}(\theta) \langle B_y|S\rangle
+
\langle S|B_n \rangle \cos^{2}(\theta) \langle B_n|S\rangle 
$$
$$ 
\langle S|A_y \rangle 
\langle A_y | B_y \rangle 
\langle B_y | A_y \rangle 
\langle A_y|S \rangle 
+ 
\langle S|A_n \rangle
\langle A_n | B_n \rangle  
\langle B_n | A_n \rangle 
\langle A_n|S \rangle  
= ...

%\langle S|B_y \rangle 
%\langle B_y | A_y \rangle  
%\langle A_y | B_y \rangle 
%\langle B_y|S \rangle
%+
%\langle S|B_n \rangle 
%\langle B_n | A_n \rangle  
%\langle A_n | B_n \rangle 
%\langle B_n|S \rangle 
$$
$$
\langle S | P_A P_B P_A | S \rangle
+
\langle S | P_\bar{A} P_\bar{B} P_\bar{A} | S \rangle
=
\langle S | P_B P_A P_B | S \rangle
+
\langle S | P_\bar{B} P_\bar{A} P_\bar{B} | S \rangle
$$
Since $P^2_x = P_x$
$$
\langle S | P_A P^2_B P_A | S \rangle
+
\langle S | P_\bar{A} P^2_\bar{B} P_\bar{A} | S \rangle
=
\langle S | P_B P^2_A P_B | S \rangle
+
\langle S | P_\bar{B} P^2_\bar{A} P_\bar{B} | S \rangle
$$
$$
|| P_B P_A |S\rangle||^2
+
|| P_\bar{B} P_\bar{A} |S\rangle||^2
=
|| P_A P_B |S\rangle||^2
+
|| P_\bar{A} P_\bar{B} |S\rangle||^2
$$
$$
p(A B )
+
p(\bar{A} \bar{B} )
=
p( B A)
+
p( \bar{B} \bar{A})
$$
Which is the QQ equality. 

You can also repeat this for the off-diagonal components to get the other version of the equality. 

# Implications/Reflections

To me, this is interesting. The original was based on a statistical description, while this is more based on symmetry and fundamentals of quantum descriptions and ties it to symmetries.

This also makes moving to questions with more options simpler, meaning we can make predictions based on the symmetry/constraint relationship. I still need to work it out, but I am looking into it.

The main problem with this proof is that it makes more assumptions than the original. In the original, they allowed the subspace corresponding to each response option to be anything, while this formulation requires that the subspaces be a line described by a single vector. I would argue that this is a reasonably innocuous assumption, equivalent to requiring the question to be well formulated and measure a single thing. 

If the subspace for a response is a plane, for instance, there are two variables that can change but give you the same measurement. In psychology, this is a double-barreled question and is considered poorly constructed. If we have a line, we can not produce the same measurements from different levels of variables. The assumption of a linear subspace is reasonable to me because it is the assumption that we are dealing with well-constructed questions. 

# Questions

First off, what are your general thoughts? Is this a reasonable and/or interesting argument? 

Secondly, how much am I reaching with my symmetry argument here? 

Thirdly, what would it take for this sort of thing to be publishable? I recognize that's not your area of expertise, but I would value your opinion. 

Finally, do you have any notes/suggestions?