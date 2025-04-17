```matlab
%CRRA utility u(c)=c.^(1-sigma)/(1-sigma)
sigma = 1;
beta = 0.97;
% E(C,z) = z * u(C),W(C,z)= beta * Q * V, V(C,z) = max{E,W}
%policy: eat if E > W
C = 100;
n_C = length(C);
z = [0.75,1.0,1.25];
n_z = length(z);

E = zeros(n_z,n_C);
W = zeros(n_z,n_C);
V = zeros(n_z,n_C);

Q = [0.90 0.05 0.05;
    0.05 0.90 0.05;
    0.05 0.05 0.90];
it_max = 1000;
it_tol = 0.00001;
for i = 1:n_z
    if sigma ==1
        E(i,:) = z(i) * log(C);
    else
        E(i,:) = z(i) * C ^ (1-sigma)/(1-sigma)
    end
end

W_0 = beta * Q * E
V_0 = max(E,W_0)

it = 1;
dif = 1000;
while (it < it_max && dif > it_tol)
    W1 = beta * Q * V_0;
    V_1 = max(E,W1);
    dif = max(abs(V_1 - V_0));
    V_0 = V_1;
    it = it + 1;
end
disp('nums of iterations')
it
V = V_1;
W = W1;

policy = (E > W);
disp("--------------------------------")
disp('shock   policy   V     E      W')
res = [z' policy V E W];
disp(res)

```

