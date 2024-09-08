def get_Item_ids_for_all_groups(access_token, group_ids, api_name, id_key =None, return_type ="json"):
    results = {}
    for group_id in group_ids:
        endpoint = f"groups/{group_id}/{api_name}"
        try:
            response_data = call_powerbi_api(access_token, endpoint)
            if isinstance(response_data, dict):
                items = response_data.get('value', [])
            elif isinstance(response_data, list):
                items = {'value': response_data}
            else:
                raise Exception(f"unexpected response format for group {group_id}")
        
            if return_type == "ids":
                if id_key is not None:
                    raise ValueError(f"id_key must be specified for return_type 'ids'")
                results[group_id] = [item.get(id_key) for item in response_data if item.get(id_key) is not None] 
                    # return [group_id for group_id, items in results.items() if items is not None]
            elif return_type == "json":
                results[group_id] = items
                        # results[group_id] = [ item for item in response_data.get("value") if item is not None]
                return results
            else:
                raise Exception(f"Invalid return type . Use 'ids' or 'json'")
        except Exception as e:
            raise Exception(f"unexpected response format for group {group_id}: {e}")

    return results


{'id': 'e26b065e-851e-4fc4-95da-4600e0f52423',
  'isReadOnly': False,
  'isOnDedicatedCapacity': False,
  'type': 'Workspace',
  'name': 'Microsoft Project Web App'},
