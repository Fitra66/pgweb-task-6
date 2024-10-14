<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Leaflet JS</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.6.0/css/all.min.css"
        integrity="sha512-Kc323vGBEqzTmouAECnVceyQqyqdsSiqLQISBL29aUW4U/M7pSPA/gEUZQqv1cwx4OnYxTxve5UMg5GT6L4JJg=="
        crossorigin="anonymous" referrerpolicy="no-referrer" />
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
        integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
    <link rel="stylesheet" href="plugin/leaflet-search-master/leaflet-search-master/dist/leaflet-search.min.css">
    <link rel="stylesheet"
        href="plugin\Leaflet.defaultextent-master\Leaflet.defaultextent-master\dist\leaflet.defaultextent.css">
    <style>
        html,
        body {
            width: 100%;
            height: 100%;
            margin: 0;
        }

        #map {
            width: 100%;
            height: calc(100vh - 56px);
        }
    </style>
</head>

<body>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
            <a class="navbar-brand" href="#"><i class="fas fa-map"></i>Penajam Paser Utara</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
                aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link" href="http://geoportal.penajamkab.go.id/" target="_blank"><i
                                class="fa-solid fa-layer-group"></i>Sumber Data</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" data-bs-toggle="modal" data-bs-target="#exampleModal"><i
                                class="fa-solid fa-circle-info"></i></i>Info</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <div id="map"></div>

    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">Info</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <table class="table table-striped">
                        <tr>
                            <th>Nama</th>
                            <td>R. Novalda Alfitra Jazma</td>
                        </tr>
                        <tr>
                            <th>NIM</th>
                            <td>23/521105/SV/23373</td>
                        </tr>
                        <tr>
                            <th>Kelas</th>
                            <td>B</td>
                        </tr>
                        <tr>
                            <th>Github</th>
                            <td><a href="https://github.com/fitra66" target="_blank"
                                    rel="noopener noreferrer">https://github.com/fitra66</a></td>
                        </tr>
                    </table>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>
    </div>

    <!-- Feature Modal -->
    <div class="modal fade" id="featureModal" tabindex="-1" aria-labelledby="featureModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="featureModalTitle" </h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body" id="featureModalBody">
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
        crossorigin="anonymous"></script>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
        integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

    <script src="plugin/leaflet-search-master/leaflet-search-master/dist/leaflet-search.min.js"></script>
    <script
        src="plugin\Leaflet.defaultextent-master\Leaflet.defaultextent-master\dist\leaflet.defaultextent.js"></script>
    <script>
        // Inisialisasi peta
        var map = L.map("map").setView([-1.2099675, 116.5704346], 13);

        // Tile Layer Base Map
        var basemap = L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
            attribution:
                '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
        });

        // Menambahkan basemap ke dalam peta
        basemap.addTo(map);

        // GeoJSON Point Sarana Prasarana
        var sarana_prasarana = L.geoJSON(null, {
            // Style

            pointToLayer: function (feature, latlng) {
                return L.marker(latlng, {
                    icon: L.icon({
                        iconUrl: "icon/home_marker.png", // icon marker
                        iconSize: [48, 48], // ukuran icon
                        iconAnchor: [24, 48], // posisi icon terhadap titik (point)
                        popupAnchor: [0, -48], // posisi popup terhadap icon
                        tooltipAnchor: [-16, -30], // posisi tooltip terhadap icon
                    }),
                });
            },

            // onEachFeature

            onEachFeature: function (feature, layer) {
                // variable popup content
                var popup_content = "Nama: " + feature.properties.SARANA_PRA + "<br>" +
                    "Koordinat: " + feature.geometry.coordinates[1] + ", " + feature.geometry.coordinates[0];

                layer.on({
                    click: function (e) {
                        sarana_prasarana.bindPopup(popup_content);

                        //Menampilkan feature modal
                        $("#featureModalTitle").html("Sarana Prasarana");
                        $("#featureModalBody").html(feature.properties.SARANA_PRA);
                        $("#featureModalBody").html(popup_content);
                        $("#featureModal").modal("show");

                    },

                    mouseover: function (e) {
                        sarana_prasarana.bindTooltip(feature.properties.SARANA_PRA, {
                            direction: "top",
                            sticky: true,
                        });
                    },
                });
            },

        });

        $.getJSON("data/sarpras.geojson", function (data) {
            sarana_prasarana.addData(data); // Menambahkan data ke dalam GeoJSON Point Sarana Prasarana
            map.addLayer(sarana_prasarana); // Menambahkan GeoJSON Point Sarana Prasarana ke dalam peta
            map.fitBounds(Sarpras.getBounds());
        });

        // GeoJSON Polyline Jalan
        map.createPane('paneJalan');
        map.getPane("paneJalan").style.zIndex = 401;
        var jalan = L.geoJSON(null, {
            pane: 'paneJalan',
            // Style
            style: function (feature) {
                return {
                    color: "red",
                    opacity: 1,
                    weight: 3,
                };
            },

            // onEachFeature
            onEachFeature: function (feature, layer) {
                // variable popup content
                var popup_content = "Fungsi: " + feature.properties.FUNGSI_JAL + "<br>" +
                    "Panjang (m): " + feature.properties.panjang_m;

                layer.on({
                    click: function (e) {
                        //jalan.bindPopup(popup_content);

                        //Menampilkan feature modal
                        $("#featureModalTitle").html("Jalan");
                        $("#featureModalBody").html(feature.properties.FUNGSI_JAL);
                        $("#featureModalBody").html(popup_content);
                        $("#featureModal").modal("show");
                    },
                    mouseover: function (e) {
                        jalan.bindTooltip(feature.properties.fungsi, {
                            direction: "auto",
                            sticky: true,
                        });
                    },
                });
            },

        });

        $.getJSON("data/jalan.geojson", function (data) {
            jalan.addData(data); // Menambahkan data ke dalam GeoJSON Polyline Jalan
            map.addLayer(jalan); // Menambahkan GeoJSON Polyline Jalan ke dalam peta
        });

        var symbologyCategorized = { "Tinggi": "#fff2e6", "Sedang": "#ffd24d", "Rendah": "#ffff10" };

        // GeoJSON Polygon Jumlah Penduduk
        map.createPane('paneJumlahPenduduk');
        map.getPane("paneJumlahPenduduk").style.zIndex = 301;
        var jumlah_penduduk = L.geoJSON(null, {
            pane: 'paneJumlahPenduduk',
            // Style
            style: function (feature) {
                return {
                    color: "BLUE",
                    opacity: 1,
                    weight: 1,
                    fillColor: symbologyCategorized[feature.properties.KELAS],
                    fillOpacity: 0.8,
                };
            },

            // onEachFeature
            onEachFeature: function (feature, layer) {
                // variable popup content
                var popup_content = "KECAMATAN: " + feature.properties.KECAMATAN + "<br>" +
                    "DESA: " + feature.properties.DESA_KELUR + "<br>" +
                    "Jumlah Penduduk: " + feature.properties.JML_PDDK_1;

                layer.on({
                    click: function (e) {
                        //jumlah_penduduk.bindPopup(popup_content);

                        //Menampilkan feature modal
                        $("#featureModalTitle").html("Jumlah Penduduk");
                        $("#featureModalBody").html("Kecamatan: " + feature.properties.KECAMATAN + "<br>" +
                            "Jumlah Penduduk: " + feature.properties.JML_PDDK_1 + "<br>" + "Desa: " + feature.properties.DESA_KELUR);
                        $("#featureModalBody").html(popup_content);

                        $("#featureModal").modal("show");
                    },
                    mouseover: function (e) {
                        jumlah_penduduk.bindTooltip(feature.properties.desa, {
                            direction: "auto",
                            sticky: true,
                        });
                    },
                });
            },

        });

        $.getJSON("data/jumlah_penduduk.geojson", function (data) {
            jumlah_penduduk.addData(data); // Menambahkan data ke dalam GeoJSON Polygon Jumlah Penduduk
            map.addLayer(jumlah_penduduk); // Menambahkan GeoJSON Polygon Jumlah Penduduk ke dalam peta
        });

        // Control Layer
        var baseMaps = {
            "Basemap": basemap,
        };

        var overlayMaps = {
            "Sarana Prasarana": sarana_prasarana,
            "Jalan": jalan,
            "Jumlah Penduduk": jumlah_penduduk,
        };

        var controllayer = L.control.layers(baseMaps, overlayMaps);
        controllayer.addTo(map);

        //Back to Default Extend
        L.control.defaultExtent()
            .addTo(map);

        //Search Control
        var searchControl = new L.Control.Search({
            layer: jumlah_penduduk,
            propertyName: 'DESA_KELUR',
            marker: false,
            moveToLocation: function (latlng, title, map) {
                //map.fitBounds( latlng.layer.getBounds() );
                var zoom = map.getBoundsZoom(latlng.layer.getBounds());
                map.setView(latlng, zoom); // access the zoom
            }
        });

        searchControl.on('search:locationfound', function (e) {

            //console.log('search:locationfound', );

            //map.removeLayer(this._markerSearch)

            e.layer.setStyle({ fillColor: '#DB7093', color: '#F5F5DC' });
            if (e.layer._popup)
                e.layer.openPopup();

        }).on('search:collapsed', function (e) {

            jumlah_penduduk.eachLayer(function (layer) {	//restore feature color
                jumlah_penduduk.resetStyle(layer);
            });
        });

        map.addControl(searchControl);  //inizialize search control

        //Logo Watermark
        L.Control.Watermark = L.Control.extend({
            onAdd: function (map) {
                var img = L.DomUtil.create('img');

                img.src = 'icon/logo SV UGM 1en.png';
                img.style.width = '200px';

                return img;
            },

            onRemove: function (map) {
                // Nothing to do here
            }
        });

        L.control.watermark = function (opts) {
            return new L.Control.Watermark(opts);
        }

        L.control.watermark({ position: 'bottomleft' }).addTo(map);



    </script>
</body>

</html>
