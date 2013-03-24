---
layout: post
title: "Be lazy with clojure"
date: 2013-03-21 15:35
comments: true
categories: clojure
---
```clojure
(defn stack-consuming-fibo [n]
  (cond
   (= n 0) 0
   (= n 1) 1
   :else (+ (stack-consuming-fibo (- n 1))
            (stack-consuming-fibo (- n 2)))))
```

```clojure
(defn tail-fibo [n]
  (letfn [(fib
            [current next n]
            (if (zero? n)
              current
              (fib next (+ current next) (dec n))))]
    (fib 0N 1N n)))
```

```clojure
(defn recur-fibo [n]
  (letfn [(fib
          [current next n]
          (if (zero? n)
            current
            (recur next (+ current next) (dec n))))]
    (fib 0N 1N n)))
```

```clojure
(defn lazy-seq-fibo
  ([]
     (concat [0 1] (lazy-seq-fibo 0N 1N)))
  ([a b]
     (let [n (+ a b)]
       (lazy-seq
        (cons n (lazy-seq-fibo b n))))))
```

```clojure
(defn fibo []
  (map first (iterate (fn [[a b]] [b (+ a b)]) [0N 1N])))
```