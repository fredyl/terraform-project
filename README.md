def get_lytx_paging_data(endpoint, page, limit, start_date=None, end_date=None):
    '''
    This function handles the pagination for the lytx API for various endpoints.
    '''
    all_data = []
    while True:
        # Initiating endpoints for pagination
        if "/vehicles/all" in endpoint:
            page_endpoint = f"/vehicles/all?limit={limit}&page={page}&includeSubgroups=true"
            process_key = "vehicles"
        elif "/video/vehicles" in endpoint:
            page_endpoint = f"/video/vehicles?PageNumber={page}&PageSize={limit}"
            process_key = "vehicles"
        elif "/video/events" in endpoint:
            page_endpoint = f"/video/events?StartDate={start_date}&EndDate={end_date}&PageNumber={page}&PageSize={limit}"
            process_key = "events"
        elif "/video/safety/eventsWithMetadata" in endpoint:
            page_endpoint = f"/video/safety/eventsWithMetadata?from={start_date}&to={end_date}&dateOption=lastUpdatedDate&sortDirection=desc&sortBy=lastUpdatedDate&includeSubgroups=true&limit={limit}&page={page}"
            process_key = None
        else:
            raise Exception(f"Invalid endpoint: {endpoint}")

        response = lytx_get_repoonse_from_event_api(page_endpoint)

        if response is None or response.status_code == 204:  # response is empty or status code is 204 stop paging
            print(f"No Content on page {page}, Stopping Pagination")
            break

        response_data = response.json()

        if response.status_code == 200:
            if process_key is not None:
                key_data = response_data[process_key]
            else:
                key_data = response_data
            print(f"Processing page {page}, with {len(key_data)} records")
            all_data.extend(key_data)
            if not key_data:
                break
        else:
            break
        page += 1  # move to next page
    print("Pagination complete")
    return all_data
