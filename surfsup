from flask import Flask, jsonify
import pandas as pd
import sqlalchemy
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session
from sqlalchemy import create_engine, func
import numpy as np
app = Flask(__name__)

engine = create_engine("sqlite:///hawaii.sqlite")
Base = automap_base()
Base.prepare(engine,reflect= True)
Measurements = Base.classes.measurments
Stations = Base.classes.stations
session = Session(engine)

@app.route('/api/v1.0/precipitation')
def precipitation():
    mes_data = session.query(Measurements).all()
    measurement_list = []
    for x in mes_data:
        prcp_list = {}
        prcp_list["Precipitation"] = x.prcp
        prcp_list["Station"] = x.station
        measurement_list.append(prcp_list)
    return jsonify(measurement_list)



@app.route("/api/v1.0/stations")
def stations():
    sta_data = session.query(Stations).all()
    station_list = []
    for y in sta_data:
        station_library = {}
        station_library["Station"] = y.station
        station_list.append(station_library)
    return jsonify(station_list)


@app.route("/api/v1.0/tobs")
def tobs():
    tob_data = session.query(Measurements).all()
    tobs_list = []
    for z in tob_data:
        tobs_library = {}
        tobs_library["tobs"] = z.tobs
        tobs_list.append(tobs_library)
    return jsonify(tobs_list)



@app.route("/api/v1.0/<start>/<end>")
def start_end(start, end):

    conn = engine.connect()
    date_and_tobs = conn.execute(" SELECT date, Tobs FROM Measurements LEFT JOIN Stations ON Measurements.station = Stations.station WHERE date > '2016-12-31' ").fetchall()
    time_df = pd.DataFrame(date_and_tobs, columns = ["Date", "Tobs"])
    time_df["Date"] = pd.to_datetime(time_df["Date"])

    st_end = time_df[(time_df["Date"] >= start) & (time_df["Date"] <= end)]
    mean = round(np.mean(st_end["Tobs"]), 2)
    minimum = np.min(st_end["Tobs"])
    maximum = np.max(st_end["Tobs"])




    return (" Start date: %s \
              End date: %s \
              Average Temp: %s \
              Max Temp: %s \
              Min Temp: %s " % (start, end, mean, maximum, minimum))






if __name__ == '__main__':
    app.run(debug=False)
