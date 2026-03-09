const errormiddleware = (err,req,res,next)=>{

  const status = err.statusCode || 500;
  const message = err.message || "Server Error";

  res.status(status).json({

    success:false,
    message:message

  });

};

module.exports = errormiddleware;