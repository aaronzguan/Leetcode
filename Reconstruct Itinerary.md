Given a list of airline tickets represented by pairs of departure and arrival airports `[from, to]`, reconstruct the itinerary in order. All of the tickets belong to a man who departs from `JFK`. Thus, the itinerary must begin with `JFK`.

**Don't need to follow lexical order, and all tickets will form a valid itinerary** -> **Must use all flights**

```python
    def get_itinerary(flights, current_itinerary):
        # Solution is complete if our itinerary uses all the flights
        if not flights:
            return current_itinerary
        
        last_stop = current_itinerary[-1]
        
        for i, (origin, destination) in enumerate(flights):
            flights_minus_current = flights[:i] + flights[i+1:]
            current_itinerary.append(destination)
            
            if origin == last_stop:
                return get_itinerary(flights_minus_current, current_itinerary)
            
            # If origin is not the last stop, the just pop the append one.
            current_itinerary.pop()
            
        return None
    
    current_itinerary = []
    current_itinerary.append('JFK')
    return get_itinerary(tickets, current_itinerary)
```