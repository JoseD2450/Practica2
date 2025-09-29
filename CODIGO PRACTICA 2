:- use_module(library(pairs)).

vehicle(toyota, corolla, sedan, 22000, 2022).
vehicle(toyota, camry, sedan, 28000, 2023).
vehicle(toyota, rav4, suv, 28000, 2022).
vehicle(toyota, hilux, pickup, 36000, 2021).

vehicle(ford, focus, sedan, 21000, 2021).
vehicle(ford, fusion, sedan, 26000, 2020).
vehicle(ford, escape, suv, 27000, 2022).
vehicle(ford, bronco, suv, 42000, 2023).
vehicle(ford, ranger, pickup, 35000, 2021).
vehicle(ford, mustang, sport, 45000, 2023).

vehicle(bmw, series3, sedan, 43000, 2022).
vehicle(bmw, x3, suv, 47000, 2021).
vehicle(bmw, x5, suv, 60000, 2021).

vehicle(honda, civic, sedan, 23000, 2022).
vehicle(honda, accord, sedan, 29000, 2023).
vehicle(honda, crv, suv, 31000, 2022).

vehicle(chevrolet, onix, sedan, 18000, 2022).
vehicle(chevrolet, traverse, suv, 34000, 2021).
vehicle(chevrolet, silverado, pickup, 43000, 2022).

vehicle(nissan, sentra, sedan, 20000, 2022).
vehicle(nissan, frontier, pickup, 33000, 2022).
vehicle(nissan, '370z', sport, 38000, 2020).

meet_budget(Reference, BudgetMax) :-
    vehicle(_, Reference, _, Price, _),
    Price =< BudgetMax.

vehicles_by_brand(Brand, Refs) :-
    findall(Ref, vehicle(Brand, Ref, _, _, _), Refs).

brand_grouped_by_type_year(Brand, Groups) :-
    setof(Type-Year-Refs,
          setof(Ref, Price^vehicle(Brand, Ref, Type, Price, Year), Refs),
          Groups), !.
brand_grouped_by_type_year(_, []).

inventory_limit(1000000).

collect_candidates(Brand, Type, Budget, CandidatesSorted) :-
    findall(ref(Ref,Price,Year),
            ( vehicle(Brand, Ref, Type, Price, Year),
              Price =< Budget
            ),
            Cand),
    sort_by_price(Cand, CandidatesSorted).

sort_by_price(List, Sorted) :-
    map_list_to_pairs(arg_price, List, Pairs),
    keysort(Pairs, SortedPairs),
    pairs_values(SortedPairs, Sorted).

arg_price(ref(_,Price,_), Price).

select_under_limit([], _Limit, [], 0).
select_under_limit([ref(R,P,Y)|T], Limit, Sel, Total) :-
    ( P =< Limit ->
        NewLimit is Limit - P,
        select_under_limit(T, NewLimit, SelRest, TotalRest),
        Sel = [ref(R,P,Y)|SelRest],
        Total is TotalRest + P
    ;   select_under_limit(T, Limit, Sel, Total)
    ).

generate_report(Brand, Type, Budget, result(List, Total)) :-
    inventory_limit(Limit0),
    OverallLimit is min(Budget, Limit0),
    collect_candidates(Brand, Type, Budget, Candidates),
    select_under_limit(Candidates, OverallLimit, List, Total).

case1_toyota_suv_under_30k(L) :-
    findall(Ref, (vehicle(toyota, Ref, suv, Price, _), Price < 30000), L).

case2_ford_grouped(G) :-
    brand_grouped_by_type_year(ford, G).

case3_total_sedan_under_500k(result(List,Total)) :-
    generate_report(_Brand, sedan, 500000, result(List, Total)).
