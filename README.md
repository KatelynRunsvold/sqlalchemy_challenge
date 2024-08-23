# sqlalchemy_challenge
i used chatgpt for help creating:
app.route("/api/v1.0/<start>")
@app.route("/api/v1.0/<start>/<end>")
def temperature_stats(start, end=None):
    session = Session(engine)
    
    start_date = dt.datetime.strptime(start, "%Y-%m-%d")
    
    if not end:
        results = session.query(func.min(Measurement.tobs), func.avg(Measurement.tobs), func.max(Measurement.tobs))\
            .filter(Measurement.date >= start_date).all()
    else:
        end_date = dt.datetime.strptime(end, "%Y-%m-%d")
        results = session.query(func.min(Measurement.tobs), func.avg(Measurement.tobs), func.max(Measurement.tobs))\
            .filter(Measurement.date >= start_date)\
            .filter(Measurement.date <= end_date).all()
    
    session.close()

    temp_stats = {
        "TMIN": results[0][0], 
        "TAVG": results[0][1], 
        "TMAX": results[0][2]
    }

    return jsonify(temp_stats)
