

Metaprogramming Snippets

# Restrict template type for function

This creates a template function MYFX(T&) where T has MYCLASS as base

    #include <type_traits>

    template<typename D, typename B>
    using extends = typename std::enable_if<std::is_base_of<B,D>::value>::type;

    template <class T, typename X = extends<MYCLASS,T> >
    void MYFX(T & p);

    
