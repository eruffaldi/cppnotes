

# Metaprogramming Snippets

## Restrict template type as derived from class

That is equivalent to a function MYFX accepting only pointers/references to a base class. This allows for a template based clone

    #include <type_traits>

    template<typename D, typename B>
    using extends = typename std::enable_if<std::is_base_of<B,D>::value>::type;

    template <class T, typename X = extends<T,MYCLASS> >
    void MYFX(T & p);


## Restrict template type to a given function signature

This is used when we want to accept a functional with given signature (function, function pointer, lambda, functor, std::function...), or check if a function can be used for given arguments

    #include <utility>
#include <iostream>

    template<class F, class...Args>
    struct is_callable
    {
        template<class U> static auto test(U* p) -> decltype((*p)(std::declval<Args>()...), void(), std::true_type());
        template<class U> static auto test(...) -> decltype(std::false_type());

        static constexpr bool value = decltype(test<F>(0))::value;
    };

    template<typename F, class...Args>
    using calleable = typename std::enable_if<is_callable<F, Args&&...>::value>::type*;

    template<class F, class...Args, typename X = calleable<F,Args...> >
    void test_call(F, Args&&...args)
    {
        std::cout << "callable" << std::endl;
    }

    template<class F, typename X = calleable<F,int,float>  >
    void neededcall(F f)
    {
        f(2,3.5f);
    }

    int main(int argc, char * argv[])
    {
        neededcall([] (int a,  float b) { std::cout << "me1 " << a << " " << b << std::endl;; } );
        neededcall([] (float a, int  b) { std::cout << "me2 " << a << " " << b << std::endl;; });
        test_call([] (float a, int  b) {}, 'a',3);

    #ifdef TEST
        test_call([] (float a, int  b, int w) {}, 1.0f,3);
        neededcall([] (float a) { std::cout << "me3 " << a << std::endl; });
    #endif
        return 0;
    }

Note: the match is not exact and it allows for cast

C++17 provides is_invocable

Modified from: http://stackoverflow.com/questions/5100015/c-metafunction-to-determine-whether-a-type-is-callable 
