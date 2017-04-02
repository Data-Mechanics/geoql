# geoql

Library for performing queries and transformations on GeoJSON data (with emphasis on support for abstract graph representations).

Package Installation and Usage
------------------------------

The package is available on PyPI:

    python -m pip install geoql

The library can be imported in the usual way:

    import geoql

An example of usage is provided  below:

    import geojson
    import geoql
    import geoleaflet
    import requests

    url = 'https://raw.githubusercontent.com/Data-Mechanics/geoql/master/examples/example_extract.geojson'
    response = requests.get(url)
    g = geojson.loads(response.text, encoding="latin-1")
    g = geoql.features_properties_null_remove(g)
    g = geoql.features_tags_parse_str_to_dict(g)
    g = geoql.features_keep_by_property(g, {"highway": {"$in": ["residential", "secondary", "tertiary"]}})
    g = geoql.features_keep_within_radius(g, (42.3551, -71.0656), 1, 'miles')
    g = geoql.features_node_edge_graph(g)
    open('example_extract.geojson', 'w').write(geojson.dumps(g))
    open('leaflet.html', 'w').write(geoleaflet.html(g)) # Create visualization.